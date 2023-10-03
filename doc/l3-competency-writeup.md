# Example Write Up for SWE L3 Competency Assessment

This write up is more applicable for Backend Engineering competency, but a similar approach can be made for other competencies.


```text

Background - Simple bullet points to help us understand the subject used for assessment
- Implementation and performance testing of Medical Balance Enquiry (MBE) API
- MBE API is an external API exposed by NPHC (National Platform for Healthcare Claims) via APEX.
- It is used by MIs (Medical Institutions) and consumed through CPF. In addition to pulling data in NPHC.
- It also needs to aggregate data from MediSave balance and MediShield Life insurance data from CPF.
- There is a 2s latency requirement for each request for NPHC, and excluding overall end-to-end latency.

Role and Achievement - Briefly describe assessee's role and the concrete deliverables so we can establish scope
- The software was developed by 3 SWEs and the performance test was performed by 2 QEs. In addition to the SWEs and QEs, we also worked with our PO from MOH, and BA from MOH IFC (GovTech Services), and 2 solution architects from GovTech SAO.
- My role was as the SWE tech lead. I worked with our stakeholders to fully define and freeze the requirements, and also our QEs and DevOps to define the testing methodology.

Key Challenges - Highlight challenges, especially technical ones encountered to give us a sensing of the level of technical difficulty
1. Requirements, particularly on performance were unclear at the beginning.
2. Difficult to coordinate with external parties (CPF) given they have separate development timelines.
3. Worked with QEs to develop the test data set and test metrics meaningful to accomplish the testing.
4. Worked with team to refactor our microservice logic to be more performant - async processing.
5. Worked with DevOps and rest of NPHC platform team to use various tools - Jaeger/OpenTelemetry, OpenSearch, Grafana to identify performance bottlenecks.

```
   
