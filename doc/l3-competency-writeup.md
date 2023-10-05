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

```

## Data Architecture

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

```
