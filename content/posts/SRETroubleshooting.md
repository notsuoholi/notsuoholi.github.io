+++
title = 'Troubleshooting as an SRE'
date = 2024-10-26T15:36:20-05:00
draft = true
+++
Below, I will outline some of best practices an SRE should follow to efficiently diagnose and resolve issues within a system, even one they are not familiar with.

## General Troubleshooting Method
### 1. The Problem Report

A problem is reported, whether by alert, customer reported ticket, or complaint/report in internal messaging service (i.e. Slack, MS Teams).

What does this problem report tell us about the expected behavior of the service? What does it tell us about the ***actual*** behavior of the service at present.
- Does it include steps I might take to reproduce the issue or is it something more vague?
- Are we tracking this report somewhere? i.e. JIRA, OpsGenie, AlertManager, etc.

### 2. Triage
Now that we have a better understanding of the report, we need to gain an understanding of the problem's severity. Is this a Sev1 or Sev2 incident? If so, it is time to declare the incident and start an incident document and log our findings and attempts at resolution, including a timeline.

If possible, delegate the task of maintaining the timeline to another person present for triage. The person(s) handling incident operations should maintain a separate document with their experiments and findings which can be linked to from the incident document.
- **sev1 - widespread outage**
- **sev2 - outage localized to a subsection of our service or degraded service performance**

If it is anything less than those, we should ensure we create a ticket and start a document where we can track findings on this particular issue.

**Note:** It is acceptable to begin taking a look at monitors, metrics, and system logs to find out the severity of an issue and to get clues on which method you might take to triage. Once you find a method for triage, it is important to apply that patch before continuing to root cause.

#### Options for Triage
Even though it is tempting to jump headfirst into investigating the issue, it is better to see how we can limit the current impact of the outage. If the issue is not currently causing any widespread issues, you may skip this step and move onto examination.

There are several options for limiting the impact of an outage. If we are experiencing an issue which affects one or several parts of our service it is wise to start at a high level by inspecting the incident report for clues as to which service(s) is most heavily affected. Strategies we might try include (but are not limited to):

- Have there been any recent changes to the service? Is it possible for us to roll them back quickly? We can try to find change windows and analyze behaviors before and after a deployment/release.
- Load Shedding & Rate Limiting
    - Rate limiting - Rejecting certain requests above a given threshold if the service is overloaded so we avoid a total failure.
- Graceful Degradation 
    - Serving degraded results if the service is overloaded so as to decrease likelihood of all out failure.
- Retry Policies
    - Limiting the retries attempted per failed requests can prevent us from getting into a cycle of `failures -> retries` which steadily increases queue and the amount of pressure put on an already overwhelmed service. Consider when retries are necessary and when they are not.
- When/Where to Retry
    - Client-side errors: These errors usually indicate issues with the request itself, such as invalid input or authentication problems, and should not be retried.
    - Server-side errors: These errors often indicate temporary issues on the server, such as overload or downtime, and are suitable candidates for retry attempts.
    - Transient errors: Errors like network timeouts or rate limiting by cloud services are often temporary and can be resolved with retries.

### 3. Examine
Once we have triaged (stopped the bleeding!) we may move to a deeper examination of the problem. We must as ourselves what this piece of the system is ***supposed*** to do? And what is it ***actually*** doing? The incident report gives us a slight clue into where the issue is as well as our triage steps.

We should now be examining:
- Service/application metrics with monitoring tools like Prometheus, Grafana, DataDog, etc. 
    - What do our error rates look like for the service? Is there an increase in query or request latency?
- Service logs for detailed views into what is happening in each the services upstream and downstream.
- Traces give us a view into the journey a particular request's journey through the system and where it ran into errors/issues.

### 4. Diagnose
- Look at the connections between components in the system. How does data normally flow between them, is there an issue with this flow?
- Use test data to check that outputs are as expected and further examine when they are not.
- Test components piece by piece, whether via a top-down approach, bisection, or a combination of both. Rule out hypothesis section by section until you have narrowed it to a few or one component.

#### Where, What, Why?
- What is the malfunctioning system doing?
- Where 

### 5. Test & Treat
#### Experiment and test.
    - Perform the quick/obvious tests first.
        - Are we failing to connect to a database? Test the connection to your DB from inside a running pod rather than looking at configuration code first.
- Document which ideas you had, how you tested them, and what the results of each test were.
    - If you have any visualizations to pair with these tests that increases the usefulness of this documentation tenfold.
Do NOT discount the value of a negative test. Getting a result we did not expect can push us closer to the correct answer and possibly surface issues we were unaware of.

#### Cure
- After narrowing down the cause(s) of the problem, we apply the fix(es) and complete our documentation (Postmortem).
- A good postmortem covers:
    - What went wrong?
    - How did we track the problem?
    - How did we fix the problem?
    - How do we prevent it from re-occurring?

## Sources
[Effective Troubleshooting](https://sre.google/sre-book/effective-troubleshooting/)
[Cascading Failures](https://sre.google/sre-book/addressing-cascading-failures/)
[Incident Management](https://sre.google/sre-book/managing-incidents/)
[Emergency Response](https://sre.google/sre-book/emergency-response/)
