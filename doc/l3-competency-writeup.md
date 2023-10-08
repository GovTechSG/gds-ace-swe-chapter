# Sample Writeups for SWE L3 Competency Assessment

The sample writeups here provide examples. Pay attention to the following.
- Clear and sufficiently limited scope to establish what you are basing the assessment on. Picking specific projects or tasks with concrete deliverables would be ideal, and these maybe reusable for performance appraisal purposes.
  - Implementation and performance testing of MediSave Balance Enquiry (MBE) API - specific API
  - Design of NPHC's claim processing DLQ (Deadletter Queue) - specific functionality of an application
  - Design and implementation of data mapping logic to facilitate data loading into NPHC from existing system
  - Performance tuning of NPHC data loader to achieve NPHC go live cutover timing requirements
- Although one writeup is needed for each competency, you can arguably reuse the same writeup as long as you can clearly separate your assessment by competency. We advise that you use a common background to describe the problem statement/project, but use specific descriptions for each competency's writeup submission.
- Pay close attention to how it helps you answer the multiple choice questions in the form as listed below.

| Question | Description |
| :- | :- |
| Scope | Level of effort for the project/tasks described in the writeup. |
| Role | Role of assessee in the described project/tasks. |
| Problem Solving | Assessee's ability in addressing technical and other challenges encountered. |
| Execution and Delivery | Assessee's ability to deliver quality output in a timely fashion. |
| Engineering Craftsmanship | Assessee's ability to promote engineering quality and standards, not just as an individual, but also at team and higher level. |


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

## Software Architecture

### Design and implementation of NPHC (National Platform for Healthcare Claims) DLQ (Dead Letter Queue)

Background - Simple bullet points to help us understand the subject used for assessment
- Event-driven claims processing flow.
- Read XML files from SFTP.
- Utilize Camel to manage the claims process and transmit claim data to multiple processes through SQS.
- Make REST calls through APEX to CPF to initiate payments.
- Forward the platform exceptions to the Dead Letter Queue (DLQ) for subsequent reprocessing once the error has been rectified.

Role and Achievement - Briefly describe assessee's role and the concrete deliverables so we can establish scope.
- Design and implementation of NPHC DLQ.
- The Platform was developed by 11 SWEs and 11 QE.
- I was the SWE lead, collaborated with MOH, IFC and the External team (CPFB, APEX, MSB, MMAE).
- My main objective was to provide architectural solutions for the platform, and I presented the proposed solution to the Solution Architect (SA).
- I designed the DLQ to manage system exceptions, enabling the system to reprocess data once the error is resolved. This enhances system stability.
- I broke down microservices based on business logic, leading to enhanced platform performance and simplified process complexity.

Key Challenges - Highlight challenges, especially technical ones encountered to give us a sensing of the level of technical difficulty. Good to throw in technical buzzwords

- Complex Claims Process:
  - The complexity of the claims process requires system design to consider performance, stability and scalability.
  - The original system dates back to 1984 and its documentation is outdated, making it challenging for PO and BA to grasp the specific business logic. The system design must be flexible to facilitate swift adaptations for document changes.
  - SWE and QE invest significant time in understanding the complex business logic, necessitating a system design that can seamlessly adapt to changing business needs.
- Performance Optimization:
  - Meeting peak demands requires the claims process to handle up to 6,000 claims per hour. Through microservices decomposition and thoughtful business process planning, processing performance has been significantly boosted from 3,000 claims per hour to an impressive 10,000 claims per hour.

  - Alleviating congestion in the SQS and enhancing information transmission efficiency is crucial. This is being achieved by disassembling SQS and integrating API calls.
- Complex Data Structure:
  - Claims data is structured within a single, sizeable JSON dataset, averaging around 100KB. Managing storage and retrieval for such data poses substantial performance challenges. Performance gains are sought by employing partition tables to expedite data processing efficiency.
  - External interfaces must efficiently access this data. Implementing multiple read-only databases has enhanced data retrieval efficiency while isolating its impact on the claims process.
- VPC Peering Milestone:
  - A significant achievement within NPHC is the pioneering use of VPC peering to establish communication with an MSB situated in the central IT services subnet. Collaboration with other teams is critical for troubleshooting APIs, especially when the gateway connectivity documentation lacks clarity.
- DLQ Design Challenges:
  - The absence of specific business requirements and application scenarios complicates DLQ design. Setting system exception capture and retry requirements relies heavily on experience.
  - Managing retries presents challenges related to data consistency. A strategy involving multiple retry nodes has been devised to prevent redundant data consumption.
  - Since the design requirements of the retry API are unclear, flexible APIs must be developed to meet different application scenarios.

## Data Architecture

### Design and Implement NPHC Data Migration Data Mapping

Background - Simple bullet points to help us understand the subject used for assessment
- Design and implementation of NPHC (National Platform for Healthcare Claims)'s Data Migration Tool
- Comprises set of ETL tools that loads existing Mediclaim data comprising 50 million claim records into NPHC before we go live.

Role and Achievement - Briefly describe assessee's role and the concrete deliverables so we can establish scope
- The software was developed by 4 SWEs and tested by 3 SWEs.
- Code was implemented in Kotlin using Spring Batch. We had parent jobs that created child jobs to be run by child workers in parallel.
- I was the SWE lead. I focused on discussing with stakeholders (our PO, BA, CPF and Mediclaim partners) to learn, understand the existing data model.
- I worked with our QE lead to develop the test plan. Both of us became subject matter expertson on our data model.
- I worked with members of the team to design and develop the algorithms to implement the loading in a performant way.
- I coordinated with our DevOps and NPHC platform team on the shared resources needed to carry out activities between the teams.
- We were able to complete our mapping to be ready for the initialial Milestone 3 launch of NPHC.
 
Key Challenges - Highlight challenges, especially technical ones encountered to give us a sensing of the level of technical difficulty. Good to throw in technical buzzwords
- Great complexity in the data:
  - Data comes from two datasources - Mediclaim database on Microsoft SQL Server, and CPF claim processing data on IBM mainframe DB2. There is overlap but they are not always in sync.
  - Compared to the Mediclaim's relational database, and CPF's hierarchical database, NPHC uses Postgres but as object database.
  - A claim includes up to hundreds of data fields, requiring deep knowledge on how the data is generated and what it should be.
  - In addition to claim data, other information - patient data, payer data, utilisation data (by patient and period, and not by claim), and many more. Even the patient or payer's IDs (usually NRIC) can change over the years and we need to be able to align them back to the same person despite ID changes.
  - Much interdependency between the data - processing of one claim depends on history of all previous claims for the same patient.
  - Data schema has evolved over decades as original system went live in 1984. It is messy. We could tell that new tables and fields were added over the years to support new functionality.
  - Business logic was complex and not well understood by the team, including even our business users. Oftentimes, we had to analyse existing data to identify patterns. 
  - Last but not least, in addition to standard claim processing, we also had to support additional workflows - reimbursement, claims that were pending manual verification, appeals, blacklists, special processing schemes. Bulk of the difficult comes from understanding the operational details so that we can start discussing how to map them to NPHC operations.
- We had to iteratively try our assumptions of the data and working collaboratively with our business users to examine the output and progressively refine our mapping model.
- Mapping of the data could not be done completely as we were both discovering the existing data, while also working with an evolving NPHC data model. Furthermore, NPHC uses a cleanroom approach to claim processing. As a result, we also had to spend considerable effort and time with our PO and soluion architects who have built our rule model to harmonise our mapping.

### Improve NPHC Data Migration Loading Performance to Meet Requirements

Background - Simple bullet points to help us understand the subject used for assessment
- Design and implementation of NPHC (National Platform for Healthcare Claims)'s Data Migration Tool
- Comprises set of ETL tools that loads existing Mediclaim data comprising 50 million claim records into NPHC before we go live.

Role and Achievement - Briefly describe assessee's role and the concrete deliverables so we can establish scope
- The software was developed by 4 SWEs and tested by 3 SWEs.
- I was the senior SWE of the team. I was focused on developing the Spring Batch foundation of the data migration tool and ensuring it runs in a performant manner.
- I proposed the use of Spring Batch to the team. I designed the initial Spring Batch structure including our parallel loading mechanism and the designt the metadata for us to track migration jobs.
- I worked with members of the team to design and develop the algorithms to implement the loading in a performant way.
- I coordinated with our DevOps and NPHC platform team on the shared resources needed to carry out activities between the teams.
- I used performance tools such as AWS Performance Insights, Grafana to monitor performance (query performance, JVM usage etc) and identify bottlenecks.
- I developed and optimised our query scripts to improve performance, and advised the rest of the team on that as I am very proficient on SQL.
- Code was implemented in Kotlin using Spring Batch. We had parent jobs that created child jobs to be run by child workers in parallel.
- We were able to complete our mapping to be ready for the initialial Milestone 3 launch of NPHC.

Key Challenges - Highlight challenges, especially technical ones encountered to give us a sensing of the level of technical difficulty. Good to throw in technical buzzwords
- We had to come up with deep optimisations in order to meet our tight timing requirements.
  - Created as many indexes as necessary to speed up queries, and also removed as many as possible to reduce update bottlenecks.
  - Created materialised views and temporary tables for us to prepare the data as early as possible, as well as to leverage the more optimised database engine over our own code implementation.
  - Used Redis to cache runtime working data.
  - Re-organised the processing to be based on each patient's claim history because of the need to have all of the patient's claims when loading each claim.
  - Used bulk insert and sequence generation.
  - Used SpringBatch multithreading step processor to have more parallelism within a single worker.
- We also overcome some of these problems.
  - We decided to simplify and not have autoscaling as our setup because Kubernetes would shutdown pods without realising that each pod is working on multi-hour requests. We should have designed for shorter-lived jobs but that only became apparent in hindsight.
  - We encountered JVM out of heap space issues so we had to ensure we had enough heap space allocated, and also watch out for memory leaks.
- Our first run of our script would have taken 3 months to migrate the data. Through extensive optimisation, we reduced it to 10 hours for 15M records 2-week delta update after pre-loading 50M for entire claim history.
