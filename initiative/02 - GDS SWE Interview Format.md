# GDS SWE Interview Format

## Rational
1. Inconsistent interviewing formats across the teams does not project an image of a cohesive organisation

## Out-of-scope
1. Building a question bank to be reused across tribes / teams. **This part of the exercise focuses specifically on the format.**

## Teams Involved
All GDS teams under ACE, DCube, and ENP tribes

## Prior Work
1. [GovTechSG/ace-hiring](https://github.com/GovTechSG/ace-hiring)

## Draft Proposed Interview Format (WIP)
The goal is to have a general interview format that potential hires will all be put through regardless of the team they apply to.

<table>
    <tr><th>Stage</th><th>Feedback / Discussion</th></tr>
    <tr>
        <td>
            <p style="font-weight:bold">Stage 0 (1 week) Issuing of Tech Challenge</p>
            <p>This serves as a first-cut to check if the candidate can implement a basic CRUD API in a language and/or framework of choice.</p>
            <p>Assessor(s) to review the candidate’s submission to decide if the candidate should proceed to stage 1</p>
        </td>
        <td>
            <ul>
                <li>First cut could use a Mettl quiz</li>
                <li>Possible to have automated test suites to validate that the tech challenge meets the requirements?</li>
                <li>CRUD API is a backend-specific tech challenge. We will need something else for frontend-focused positions</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>
            <p style="font-weight:bold">Stage 1 (1 hour) Run-through and Extension of Tech Challenge Submission</p>
            <p style="font-weight:bold">1 interviewer (possible +1 for training purposes)</p>
            <p style="font-style:italic">Assessor's Objective(s)</p>
            <ul>
                <li>Ascertain that the candidate owns his/her submission</li>
                <li>Gather evidence for competencies in the SWE Competency Framework metrics</li>
                <li>Validate the candidate’s software engineering proficiency and practices</li>
            </ul>
            <p style="font-style:italic">Suggested Format</p>
            <ul>
                <li><span style="text-decoration:underline">20 min</span>: Candidate to walk through the tech challenge submission and explain notable design decisions. Assessor to validate that the candidate has some depth of understanding of his/her own submission’s design, e.g. possibly by requiring the candidate to debug something</li>
                <li><span style="text-decoration:underline">30 min</span>: Extending the tech challenge submission by implementing a new feature. Independent or pair-programming can be done here. The problem statement should be crafted such that the candidate needs to meaningfully choose appropriate data structures and avoid unnecessarily inefficient algorithms, e.g. sets vs lists vs maps, avoid inappropriate nested loops (whether explicitly or implicitly, e.g. via the spread operator in JavaScript)</li>
            </ul>
        </td>
        <td>
            <ul>
                <li>Having 1 interviewer for 1 hour per stage for 2 stages vs 2 interviewers in a single 1 hour stage allows for independent assessments of the candidate and time to gather sufficient and accurate evidence for justifying hiring at a particular grade</li>
                <li>What is the pair-programming model used by the various teams? Should that be standardised as well?</li>
                <li>1 hour is very tight for building a new feature or thoroughly run through a system design, which might be too small to be meaningful. Suggestion to make it 1.5 hours per stage</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>
            <p style="font-weight:bold">Stage 2 (1 hour) System Design</p>
            <p style="font-weight:bold">1 interviewer (possible +1 for training purposes)</p>
            <p style="font-style:italic">Assessor's Objective(s)</p>
            <ul>
                <li>Get the candidate to design a software system that integrates multiple components.</li>
                <li>The candidate should demonstrate good ability in discussing technical details of software systems, including but not limited to evidence of critical thinking, good communication and stakeholder management skills, time management skills, and more.</li>
                <li>If there is a perceived mismatch between the candidate’s indicative grade and the evidence of competencies gathered in Stage 1, the assessor should prioritise steering the interview towards rectifying that mismatch, e.g. by requiring the candidate to quickly scope out and implement a MVP to obtain a second opinion of his/her engineering competencies.</li>
            </ul>
            <p style="font-style:italic">Suggested Format</p>
            <ul>
                <li>Either a 60 minute discussion based on the given scenario, or a 40 minute discussion followed by a 20 minute building of the MVP</li>
                <li>The problem should be introduced from the role of a PO / BA, with requirements sufficiently ambiguous to provoke discussion. The example tech challenge below can be adapted for use in this stage.</li>
            </ul>
        </td>
        <td>
            <ul>
                <li>May not be suitable for fresh / junior candidates and possibly should be restricted to indicative grades of G and up</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>
            <p style="font-weight:bold">Stage 3 (45 min) Behavioural / Culture-Fit Interview</p>
            <p style="font-style:italic">Assessor's Objective(s)</p>
            <ul>
                <li>Gather evidence of the candidate’s competencies in the Leadership Competency Framework metrics</li>
            </ul>
            <p style="font-weight:bold">Currently handled by DMs</p>
        </td>
        <td>
        </td>
    </tr>
</table>

## Example of Tech Challenge for Mid / Senior Candidates

---

### Problem Statement

There is frequently a need for teams to collectively decide on a location to head to for lunch. While each team member has an idea in mind, not everyone gets heard in the commotion and much time is spent to arrive at what may as well be a random choice.

### Task

Design and build an application that meets the following requirements.

1. A user can initiate a session and invite others to join it.
2. Other users who have joined the session may submit a restaurant of their choice.
3. All users in the session are able to see restaurants that others have submitted.
4. The user who initiated the session is able to end the session. 
  a. At the end of a session, a restaurant is randomly picked from all submitted restaurants. All users in the session are then able to see the picked restaurant.
  b. A user should *not* be able to join a session that has already ended.

### Expected Engineering Qualities

We are looking to observe the application of good software engineering practices, so you should consider this as an opportunity to showcase what quality software means to you and how you would achieve it. The following is a non-exhaustive example list of questions we hope to see answered from reading your code:

1. How would you ensure that one user’s submitted location does not cause issues on other users’ displays?
2. How would you assure others that your application meets the requirements, and continues to meet the requirements even after changes have been made to it?
3. How would you ensure that your application can be deployed to serve an increasing number of users who may be concurrently using your service?
4. How would you help a fellow team member who may need to debug your application in future?

Feel free to extend the application with salient features as long as it continues to serve the users’ purpose.

### Expected Artefacts

1. Commit your code to a github repository and send its link to us via email.
2. Your repository should include a document explaining how your application is expected to be cloned and run, including any necessary preparations of the environment.
3. Any APIs that have been developed as part of your application should also be documented.
4. Significant design decisions should also be documented to facilitate future discussions.

---

### Example Assessment Rubrics

(WIP)

### Next Order Issues

1. What questions to craft?
2. How do we map to technical competencies?
3. How do we map to the LCF?

