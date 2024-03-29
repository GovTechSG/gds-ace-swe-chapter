# Sample Writeups for SWE L3 Competency Assessment

Here are a few writeups to provide examples for the "Basis of Assessment" field in the form.
1. Backend Engineering - Implementation and performance testing of MediSave Balance Enquiry (MBE) API - specific API
2. Software Architecture - Design of NPHC's claim processing DLQ (Deadletter Queue) - specific functionality of an application
3. Data Architecture - Design and implementation of data mapping logic to facilitate data loading into NPHC from existing system
4. Data Architecture - Performance tuning of NPHC data loader to achieve NPHC go live cutover timing requirements

Tips
- Pick specific deliverables rather than overall project or product work. We are best served by limited scope and well-defined deliverables for which we can easily assess complexity. A broad writeup of assessee's overall work accomplishments is to broad and vague.
- Feel free to throw in technical terms as long as those are actually used in the delivereables.
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

## Implementation and performance testing of MediSave Balance Enquiry (MBE) API

Background - Simple bullet points to help us understand the subject used for assessment
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

### Update Design and Optimise NPHC (National Platform for Healthcare Claims) Design for Performance

Background - Simple bullet points to help us understand the subject used for assessment
- NPHC is Singapore's national system to process all MediSave and MediShield Life claims. It implements event-driven workflow that processes files received and delivered via SFTP.
- Claim processing is implemented as a collection of microservices, utilising Apache Camel workflow and integration framework, and communicating via AWS SQS.
- IBM ODM (Operational Decision Manager) is used for us to implement complex claim processing rules and accessed via REST API.
- We also need to integrate with external parties/application via SFTP and REST to fully implement the claim processing flow - payments, patient information, medical institution accreditation etc. If Medishield Life is involved, claims also need to be routed to external insurers for additional processing.
- We developed our intitial solution, but had to make adjustments to our software design to meet performance requirements before going live -up to 60,000 claims per hour.

Role and Achievement - Briefly describe assessee's role and the concrete deliverables so we can establish scope.
- The entire NPHC Claim Processing Platform was developed by 11 SWEs and 11 QE.
- I was the SWE lead, collaborated with MOH, IFC and the External team (CPFB, APEX, MSB, MMAE).
- My main objective was to provide architectural solutions for the platform, and I presented the proposed solution to the Solution Architect (SA).
- I redesigned the microservices to improve the performance of the platform in processing claims.
- I broke down microservices based on business logic, leading to enhanced platform performance and simplified process complexity.

Key Challenges - Highlight challenges, especially technical ones encountered to give us a sensing of the level of technical difficulty. Good to throw in technical buzzwords
- Complex Claims Process:
  - The original platform dates back to 1984 and its documentation is outdated, making it challenging for PO and BA to grasp the specific business logic. The platform design must be flexible to facilitate swift adaptations for document changes.
  - SWE and QE invest significant time in understanding the complex business logic, necessitating a platform design that can seamlessly adapt to changing business needs.
- Performance improvement Challenges:
  - The initial platform design mainly focused on functional implementation and lacked stress testing.
  - Given the complexity of platform functionality, comprehensive integration testing is required to identify performance bottlenecks.
  - Improving platform performance requires a comprehensive transformation of the entire platform architecture, and ensuring platform stability is the top priority.
- Complex Data Structure:
  - Ensuring valid external interfaces to access this data is critical. The introduction of multiple read-only databases not only improves data retrieval efficiency, but also effectively reduces its impact on the claims process.
  - The claims data resides in a fairly large JSON dataset, averaging around 100KB. Managing the storage and retrieval of such data presents significant performance challenges. To achieve significant performance improvements, we use partitioned tables to simplify data processing. This optimization resulted in a significant reduction in search time, from 6 seconds to just 2 seconds.
- Complex Microservice Process:
  - Meeting peak demand requires the claims process to process up to 6,000 claims per hour. Recognizing the inefficiencies in the current claims process, we began making changes to the microservices design. Specifically, we partition resource-intensive microservices to improve performance and consolidate microservices with lower resource requirements. Through this microservices re-engineering initiative, combined with careful business process planning, we achieved significant improvements in performance, increasing processing capacity from 3,000 claims per hour to an impressive 10,000 claims per hour.
  - Some microservices experience heavy information processing, causing SQS congestion. It is crucial to alleviate this congestion and improve the efficiency of information transmission. We solved this problem by converting from SQS to API calls for data transfer. This shift significantly improved data transfer efficiency without affecting data reliability, ultimately reducing the number of microservice Pods from the original 12 to 6 while maintaining performance standards.
- Audit Log Writing Efficiency
  - Large amounts of audit logs pose challenges for efficient writing. Although transferring logs through SQS can speed up log writing efficiency, it also brings cost considerations. To this end, we implemented AWS's Kinesis service for asynchronous audit log submission and secure backup in S3, ensuring efficient operations and cost management.
 
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
- We were able to complete our mapping and implement the migration tool to be ready for the initialial Milestone 3 launch of NPHC.
 
Key Challenges - Highlight challenges, especially technical ones encountered to give us a sensing of the level of technical difficulty. Good to throw in technical buzzwords
- Very different systems design: NPHC is a cleansheet design with rules implemented from the ground up. We could not directly port over the existing data, but instead had to recreate it. We spent considerable effort with our PO and soluion architects to discuss and harmonise our mapping.
- Great complexity in the data:
  - Data comes from two datasources - Mediclaim database on Microsoft SQL Server, and CPF claim processing data on IBM mainframe DB2. There is overlap but they are not always in sync.
  - A claim includes up to hundreds of data fields, requiring deep knowledge on how the data is generated and what it should be.
  - Other information - patient data, payer data, utilisation data (by patient and period, and not by claim), and many more. Even the patient or payer's IDs (usually NRIC) can change over the years and we need to be able to align them back to the same person despite ID changes.
  - We cannot just migration each claim indidivudally. A claim's state depends on history of all previous claims for the same patient.
  - Data schema has evolved over decades as original system went live in 1984. It is messy. We could tell that new tables and fields were added over the years to support new functionality.
  - Business logic was complex and not well understood by the team, including even our business users. Oftentimes, we had to analyse existing data to identify patterns. 
  - There are additional claim processing flows to support - reimbursement, claims that were pending manual verification, appeals, blacklists, special processing schemes.
- We had to try our assumptions, and working collaboratively with our business users to examine the output and progressively refine our mapping model.

### Improve NPHC Data Migration Loading Performance to Meet Requirements

Background - Simple bullet points to help us understand the subject used for assessment
- Design and implementation of NPHC (National Platform for Healthcare Claims)'s Data Migration Tool
- Comprises set of ETL tools that loads existing Mediclaim data comprising 50 million claim records into NPHC before we go live.

Role and Achievement - Briefly describe assessee's role and the concrete deliverables so we can establish scope
- The software was developed by 4 SWEs and tested by 3 SWEs.
- I was the senior SWE of the team. I was focused on developing the Spring Batch foundation of the data migration tool and ensuring it runs in a performant manner.
- I proposed the use of Spring Batch to the team. I designed the initial Spring Batch structure including our parallel loading mechanism and the designt the metadata for us to track migration jobs.
- I used performance tools such as AWS Performance Insights, Grafana to monitor performance (query performance, JVM usage etc) and identify bottlenecks.
- I developed and optimised our query scripts to improve performance, and advised the rest of the team on that as I am very proficient on SQL.
- Code was implemented in Kotlin using Spring Batch. We had parent jobs that created child jobs to be run by child workers in parallel.
- We were able to complete our mapping to be ready for the initialial Milestone 3 launch of NPHC.

Key Challenges - Highlight challenges, especially technical ones encountered to give us a sensing of the level of technical difficulty. Good to throw in technical buzzwords
- We had to come up with deep optimisations in order to meet our tight timing requirements.
  - Created materialised views and temporary tables for us to prepare the data as early as possible, as well as to leverage the more optimised database engine over our own code implementation.
  - Used Redis to cache runtime working data.
  - Re-organised the processing to be based on each patient's claim history because of the need to have all of the patient's claims when loading each claim.
  - Used bulk insert and sequence generation.
  - Used Spring Batch multithreading step processor to have more parallelism within a single worker.
- We also overcome some of these problems.
  - We decided to simplify and not have autoscaling as our setup because Kubernetes would shutdown pods without realising that each pod is working on multi-hour requests. We should have designed for shorter-lived jobs but that only became apparent in hindsight.
- Our first run of our script would have taken 3 months to migrate the data. Through extensive optimisation, we were able to reduce it to 10 hours for 15M records 2-week delta update after pre-loading 50M for entire claim history.
