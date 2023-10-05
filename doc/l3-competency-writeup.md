# Sample Write Ups for SWE L3 Competency Assessment

The write up is meant for evaluating technical competency and not for review. While we want to understand the scope and complexity done, we only need enough to assess competency, and not to recognise work accomplishments.

We have it in monospace text format to match what you need to type in the form.

## Backend Engineering

Background - Simple bullet points to help us understand the subject used for assessment

- Implementation and performance testing of MediSave Balance Enquiry (MBE) API
- MBE API is an external API exposed by NPHC (National Platform for Healthcare Claims) via APEX.
- This API replaces an existing API to query the current system running on a mainframe.
- This is consumed by MIs (Medical Institutions) and Partner C.
- It needs to aggregate
    - MediSave balance and MediShield Life insurance data from Partner C
    - Amalgamation and utilisation from NPHC microservices
    - Codes and appeal information from Partner A

Scope and Deliverables - Briefly describe your role and concrete deliverables, so we can establish scope

- My role was as the SWE tech lead.
- I led a team of 5 SWEs and 5 QEs (3 for testing and 2 for performance testing). As well as work with POs from MOH, BA from MOH IFC (GovTech Services), and 2 solution architects from GovTech SAO.
- I worked with our stakeholders to fully define and freeze the requirements, and also our QEs and DevOps to define the testing methodology for both the requirements and performance testing.
- API is exposed in an integration via Partner B -> APEX.
- Performance requirement is 40 transactions per second (TPS) for 1 hour with a 3s max response time for 95% of requests.
    - Exit Criteria: < 2% failure rate, < 80% CPU and memory utilisation

Key Contributions - Highlight key challenges solved, especially technical ones encountered to give us a sensing of the level of technical difficulty

1. Requirements, particularly on performance were unclear at the beginning.
2. Difficult to coordinate with external parties (Partner A, Partner B, Partner C) given they have separate development timelines.
3. Worked with team to refactor our microservice logic to be more performant - by using async processing and bulk queries. Because of this, we had to restructure the way the code prepares and calls the external APIs.
    - I created a base for the handler and taught the team to use the structure for all the calls as well as shared the trade-offs with using this method for optimisation. Each processing code was split into 2 parts so that we can prepare the input before making the concurrent calls.
    - Added bulk queries to reduce the number of calls to external services.
    - Tuned the code by adjusting parameters such as maxRequestsPerHost and using connectionPools.
4. Worked with QEs to develop the test data set and test metrics meaningful to accomplish the testing.
5. Worked with DevOps and rest of NPHC platform team to use various tools - Jaeger/OpenTelemetry, OpenSearch, Grafana to identify performance bottlenecks.
6. Previously, the API was hitting 15s timeout at 10 TPS. Successfully tuned the API to meet the performance requirement and submitted a report with QE on the tuned API hitting performance test targets as well as subsequent performance tests.
```

## Software Architecture

Background - Simple bullet points to help us understand the subject used for assessment
- Design and implementation of NPHC (National Platform for Healthcare Claims) D
- MBE API is an external API exposed by NPHC (National Platform for Healthcare Claims) via APEX.
- It is used by MIs (Medical Institutions) and consumed through CPF. In addition to pulling data in NPHC.
- It also needs to aggregate data from MediSave balance and MediShield Life insurance data from CPF.
- There is a 2s latency requirement for each request for NPHC, and excluding overall end-to-end latency.

Role and Achievement - Briefly describe assessee's role and the concrete deliverables so we can establish scope
- The software was developed by 3 SWEs and the performance test was performed by 2 QEs. In addition to the SWEs and QEs, we also worked with our PO from MOH, and BA from MOH IFC (GovTech Services), and 2 solution architects from GovTech SAO.
- My role was as the SWE tech lead. I worked with our stakeholders to fully define and freeze the requirements, and also our QEs and DevOps to define the testing methodology.

Key Challenges - Highlight challenges, especially technical ones encountered to give us a sensing of the level of technical difficulty. Good to throw in technical buzzwords
1. Requirements, particularly on performance were unclear at the beginning.
2. Difficult to coordinate with external parties (CPF) given they have separate development timelines.
3. Worked with QEs to develop the test data set and test metrics meaningful to accomplish the testing.
4. Worked with team to refactor our microservice logic to be more performant - async processing.
5. Worked with DevOps and rest of NPHC platform team to use various tools - Jaeger/OpenTelemetry, OpenSearch, Grafana to identify performance bottlenecks.

## Data Architecture

Background - Simple bullet points to help us understand the subject used for assessment
- Design and implementation of NPHC (National Platform for Healthcare Claims)'s Data Migration Tool
- Comprises set of ETL tools that loads existing Mediclaim data comprising 50 million claim records into NPHC before we go live.

Role and Achievement - Briefly describe assessee's role and the concrete deliverables so we can establish scope
- The software was developed by 4 SWEs and tested by 3 SWEs.
- My role was as the SWE tech lead. I was focused on ensuring proper mapping of the data by working closely with our business stakeholders, while also working closely with other team members to design the right algorithms used to improve performance.
- I also worked closely with QEs for all of us to better understand the data mapping and to develop sufficient testing for the migration.

Key Challenges - Highlight challenges, especially technical ones encountered to give us a sensing of the level of technical difficulty. Good to throw in technical buzzwords
1. Great complexity in the data:
   2. Data comes from two datasources - Mediclaim database on Microsoft SQL Server, and CPF claim processing data on IBM mainframe DB2.
   2. A claim includes up to hundreds of fields of data.
   2. It is not just claim data, but also other information - patient data, claim utilisation data (claim payout is dependent on cummulative utilisation by period), as well as other data needed for claim processing - whether patient is on black list or white list, or claim was in pending processing state while migration was in place etc. The patient or payer's IDs (usually NRIC) can also change over the years and we need to be able to align them back to the same person despite ID changes.
   2. Much interdependency between the data - processing of one claim depends on history of all previous claims for the same patient.
1. Business logic was complex and not well understood by the team, including even our business users. Oftentimes, we had to analyse existing data to identify patterns.
1. Mapping of the data could not be done completely as we were both discovering the existing data, while coordinating how to map it to evolving NPHC design at the same time.
1. Code was implemented in Kotlin using Spring Batch. We had parent jobs that created child jobs to be run by child workers in parallel.
1. We dived deep into the data to come up with algorithms to help us process as quickly as possible.
   2. Created as many indexes as necessary to speed up queries, and also removing as many as possible to reduce update bottlenecks.
   2. Created materialised views and temporary tables to prepare the data as early as possible, as well as to leverage the more optimised database engine over our own code implementation.
   2. Used Redis to cache runtime working data.
   2. Re-organised the processing to be based on each patient's claim history because of the need to have all of the patient's claims available in order to process each one of them.
   2. Used bulk insert and also sequence generation as much as possible.
   2. Used SpringBatch multithreading mode to have more parallelism within a single worker.
1. We also overcome some of these problems.
   2. We decided to simplify and not have autoscaling as our setup caused Kubernetes to shutdown workers.
   2. We encountered JVM out of heap space issues so we had to ensure we had enough heap space allocated.
1. Our first run of our script would have taken 3 months to migrate the data. Through extensive optimisation, we reduced it to 10 hours for 15M records 2-week delta update after pre-loading 50M for entire claim history.
