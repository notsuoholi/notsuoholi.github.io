+++
title = 'SRE, Core Principles'
date = 2024-10-16
draft = false
+++
I have outlined a few of the core responsibilities and practices of Site Reliability Engineers (SREs).

My goal was to cover key topics like system reliability, incident management, the importance of SLIs, SLOs, and SLAs, and how SREs use monitoring and alerting to maintain service performance while collaborating closely with development teams to ensure scalable, predictable, and efficient systems.

## SRE - A Focus Operational Resilience & TOIL Reduction
- SREs focus on system reliability and incident management. We make data driven decisions which benefit the system. We automate to reduce toil and improve change management to ensure processes in place are repeatable, predictable, and most importantly, scalable.

- We operate the 'core' of whatever software we are supporting. Can speak to the development teams about the software about what is working and what needs to be optimized. This is driven by the data we get out of the system we monitor.

- Measure success and success of the platform with things Like SLIs measured against expectations SLOs/SLAs.
- Areas of focus are:
     - Software engineering - Creating tools or frameworks, automating tasks, improving features.
     - Systems Engineering - Configuring production systems in ways which produce lasting improvement from single effort. i.e. Improving monitoring setup, server configuration, load balancer setup.
     - Reducing TOIL - Removing work that is manual, repetitive, automate-able, tactical, devoid of enduring value, and scales linearly alongside the service.
     - Overhead - The administrative work related to running a service. Meetings, paperwork, trainings, etc.

- Our primary responsibility is to the system and its reliability. We ensure the product's features do not degrade existing infrastructure by causing new software issues like bugs, introducing security risks, or increased failure rates.

## Eliminating ~~TOIL~~
- If a human has to touch it, it's a problem.

- Manual - Manually running scripts rather than automating them.
- Repetitive - Solving the same problem repeatedly rather than solving new problems or creating solutions.
- Automate-able - If a machine could do this task rather than a human, the task is considered toil.
- Tactical - Responding to a page is toil. The more we can self-heal and automate, the less pages we encounter.
- No Enduring Value - If the service is not improved by the task completed, if we're simply returning to the baseline, this task is toil.
- O(n) == Service Growth - If the task in question scales up linearly (in a 1:1 fashion) with the service, then it is likely toil. If we are operating and designing in an ideal way, a process should be able to scale without very much additional work aside from adding additional resource capacity.

- Overhead != TOIL
     - Meetings, paperwork, company meetings, code reviews, training, etc. Do not equate to toil. These things are necessary parts of running a service. Just ensure they are all producing benefit.

## Incident Management & RCA
The following are pillars of a solid incident management process.

### Recursive Separation of Responsibility
Ensure everyone on the call or incident knows their roles and plays it effectively. There are certain delegations the leader must designate in order to handle incidents well.

- Incident Commander - knows high-level state of incident. They structure the task fore and assign responsibilities according to need and priority. If you do not delegate a position, you own it.

- Operation Work - The operational leader works with the commands to respond to the incident using operational tools to resolve the problem at hand. They should be the only ones, aside from the commander, to modify the system.

- Communication - This person leads is the public face of the incidence response team. They give updates about the system to the response team and stakeholders. They may also keep the incident document updated with a timeline and actions.

- Planning - this role deals with the long term issues, filing bugs, arranging handoffs, and tracking how system diverged so it can be rectified once incident is resolved.

### Where to handle the incident?
Typically incidents require resolution inside a "war room" (or video call when remote) when incited by large enough outages. For smaller incidents, it may be appropriate to collaborate over a live document or over Slack or email. You may also work with the organization's IRC (Incident Response Consortium) to respond to the event.

### When to Declare an Incident
- The earlier the better. Even if it turns out to be a small incident, that is better than waiting too long to use the incident management framework and have the issue spiral.
- Set clear incident declaration guidelines:
     - Do you need to involve another team or several persons to fix the problem?
     - Is the outage visible to customers?
     - Is the issue unsolved even after giving several minutes (30) analysis?

### Best Practices:
- Prioritize - stop the bleeding, restore the service, preserve evidence for RCA.
- Prepare - develop and document incident response and management in advance.
- Trust - trust your team, give each role autonomy required
- Introspect - Pay attention to feelings during response. If overwhelmed, seek additional help and delegate
- Consider alternatives - does what you're currently doing make sense? take a step back and try alternatives.
- Practice - get comfortable with the process
- Change it up - try all hats, be comfortable with handling multiple roles during an incident

### RCA Best Practices and Postmortems
The goal of any useful postmortem is to ensure that the incident is well documented ***and*** that all underlying root causes are well understood and, that effective actions are implemented to reduce likelihood and impact of recurrence.

RCA Steps:
- Define your problem or symptom
- Identity possible causes
- Validate the identified causes

### Handoff
- If you must leave or end your shift, hand off the incident to the new commander. They should be aware of all actions taken, what is to be done next, and be clear that they are not in command.

## Monitoring & Alerting
When to bother a human and how to deal with issues that don't need one.

- White-box: Monitoring based on metrics exposed by system internals. i.e. logs, http handlers emitting internal stats. Can tell us when we're trending toward problems.
     - There is a problem within our system. It may be affecting users right now ***or*** it will impact them soon if we don't do something about it.
     - Can be symptom or cause (depending on system(s) affected). Proactive.

- Black-box: Externally visible behaviors as users might see it. Tells us when there is currently a problem visible to our end users.
     - There is something wrong with the application right now. i.e. Users cannot load specific paths. We need to respond.
     - Diagnoses the symptom. Reactive.

### Why monitor?
- Analyzing long-term trends - How are parts of your application performing or growing over time?
- Comparing over time to experimental groups - Will this new feature improve or degrade the application's historic performance?
- To alert - Something is not working as we expect. We should either fix ASAP or create a ticket if it is not currently broken.
- Dashboards - Answer basic questions about our service. Include some form of Golden signals.
- Ad-hoc retrospective analysis - why did 500 error rates just shoot up? what else happened at the same time?

### Humans when?
- If our system cannot self-heal the specific issue (i.e., auto-scale when traffic is at peak), human intervention is needed.

### Keep it quiet
- Noise should be low, signal should be high. If it's connected to a pager it should be simple, yet robust. If we alert a human it should be for a clear and very important reason. It should also, ideally, give the human some guidance for resolution whenever possible.

### Symptoms vs Causes
A monitoring system must address what is broken (the symptom) and why (the cause).
Examples:
| Symptom  | Cause   |
| -------- | ------- |
| Increase in 500's  | DB servers are refusing connections   |
| Response times slow | CPU overload or system oom killing too many pods     |
| Users in Idaho cannot load your application    | Blacklisted some client IPs |

### The Four Golden Signals
- Latency - time it takes to serve a request. Think of something like request duration for nginx ingress.
- Traffic - measure of how much demand is being placed on your system. So how many requests are we processing for a given service per second?
- Errors - rate of requests that fail explicitly (like 500s), implicitly (like 200s coupled with incorrect content/path), or policy (request came back above acceptable time limit)
- Saturation - how “full” a service is. How many resources are being constrained on the service at a given time. Latency increases are often a marker for saturation of a service.

### Worry about your tail
- Measure things like latency in buckets rather than collecting averages or means. 
- In order to differentiate between a slow average vs a slow "tail" of requests is to collect them in buckets by latency. 
- The distribution (coupled with 50-99th percentiles) can tell you a lot more.

### KISS (Keep it simple, s....)
 - Rules for spotting incidents must be simple, predictable, and reliable.
 - Outdated and/or unused configurations should be removed from our system.
 - Signals we collect but don't use might need to be removed as well.

### When creating monitors and alerts, remember
- Does this rule detect an undetected condition which is urgent, actionable, or visible to the user?
- Is this alert ignorable? Why is it an alert?
- Does this alert properly reflect what users are experiencing?
- Can I take some type of action based on this alert?
     - Could this action be automated?
     - Is this action permanent or temporary?
- Are others being paged in addition or myself?
     - Can we reduce the number of people paged unnecessarily?
- Pagers mean urgent action. There is only energy for so much urgent action in a day before fatigue sets in.
- If it is a page, it must be actionable.
- Page responses should be calls for intelligent action.
     - If the response if robotic (or scripted), it should be automated.
- Pages should reflect novel problems or event that have never been seen.

### Long Term Results of Monitoring
- If you are finding certain alerts begin to fire often, it is time to determine the root cause and fix it. If it is not something you can fix, automate it.

- Prioritize long-term fixes even if it means short-term decreases in availability. Patches simply become tech-debt when teams are unable to prioritize real fixes for these issues.

- Consider keeping a log of all pages per quarter to get an idea of possible page fatigue in a team or organization.

## SLI -> SLO -> SLA (& Error Budgets)
- The basic properties of metrics that matter to us, what values we want them to have, and how we react if we can't provide the level of service we expect. These help us to determine our service is healthy and delivering overall performance expected from our customers.

- SLIs are a carefully defined, quantitative measure of some piece of the service. i.e. request latency, error rate, availability collected over some measurement window and turned into an average, rate, percentile, etc.
     - latency - how long does it takes to return a response to a request
     - error rate - a fraction of all request received with non-successful status code
     - system throughput - requests per second
- Availability is the fraction of time that a service is usable. Can be measure in terms of yield (well-formed requests which succeed).
     - success measured in terms of 9's we can achieve (100% is impossible).
     - 99.9-99.9999.
- SLIs feed into SLOs which are what we set as our target or "objective" for our SLIs. Our SLO might be that we want to deliver a request latency or less than 100ms for 90% of our users at all times. We need to ensure our SLI's on on pace to meet that requirement. 
     - We cannot always set an SLO as some things are determined by the user. (Queries per second is purely determined by the user)
     - A user starts to perceive latency around 100ms so we want to keep our times below that whenever possible.
- SLAs implicit or explicitly contracts with end users that include consequences for meeting or missing the SLOs they contain. Consequences can be financial or even reputation damaging.
- How to tell the difference? If there is no explicit consequence for missing an SLO then it is only an SLO, not an SLA.

### What do we and our users really care about?
- Every metric we track will not be an SLI. When we understand what our user want, we can determine our SLIs. 
- Too many indicators lead to lack of attention and too few lead to lack of understanding of system health and performance.
- Start backwards. Think about your objectives and develop your SLIs to feed into it.

- Typical Measures:
     - User-facing systems (like a web frontend): availability, latency, throughput. Could we respond? How long did it take? How many requests did we handle?
     - Storage systems: latency, availability, durability. How long does it take to read/write data? Is the data accessible right when we need it? Is the data ***still there*** when we need it?
- What should all systems care about?
     - Correctness. Was the right answer returned? Was the right data retrieved?

### Measures
- Remember to also measure on the client-side. Ensure we collect data from client instead of just focusing on the backend. i.e. How long does it take for a page to become usable in a user's browser? This may indicate problems in my JS if it's slower than expected.

### Aggregating data
- While the raw data we collect may be simple (requests /s), the way we aggregate this data must be done carefully.
     - How are we aggregating this data? Do we obtain it once per second or by averaging requests over a full minute? If it's once per second we may be hiding high instantaneous bursts in request rates that last for only seconds at a time.
     - For latency, averaging requests may be appealing but obscures the fact that it is possible for ***most*** requests to be fast but a long tail of requests to be much slower, making latency overall seem higher than is true.

- It is better to think of metrics as distributions than averages.
     - Latency SLIs would be better served this way, using percentiles as indicators rather than averages.
     - 99th percentile - the plausible worst cases, if the 99th percentile is acceptable then user experience is likely good. The same if it is bad.
     - 50th percentile (the median) - the typical case

- Keep your SLI definitions standard!
     - Ensure you have defined your measures like aggregation intervals, regions, how frequently measures are made, and data-access latency in a way that can be accepted and used by others.
     - Saves time understanding what the heck you're looking at for each new indicator.
     - Build a set of reusable templates for your most common metrics as it makes it easy for others to implement.

### Error Budgets
It is not possible (or wise) to attempt to meet SLOs 100% of the time. If we did that, it would mean much more rigid rules for operation and slow our ability to innovate and iterate.

- Error budgets allow us to accept some level of "failure" in exchange for our ability to move with pace and also deliver value to our customers. These rates at which our SLOs can be missed are tracked on daily -> monthly -> yearly basis.
- Ensure that you do not agree to error budgets which are too low, otherwise you may end up violating your SLOs.
- Develop them in relation to the uptime you desire for your service.
     - How much uptime should you have per quarter? 
     - Ensure your monitoring system can effectively measure your uptime. 
     - How much `unreliability` can we accept in a given quarter before it negatively impacts our customers?
     - Develop your `budget`based on the above factors. Then you can manage your release cycle around it.

### Determining Availability & Error Budgets
- Target level of availability for services should be determined based on:
     - What do users expect of the service?
     - Does this service have direct effect on my revenue?
     - Is the service free or paid?
     - Are there competitors in the marketplace and what is their level of service?
     - Is this service for consumers or enterprises?
#### Error Budget Calculations:
`time-based availability` = `uptime / (uptime + downtime)`
- [Availability table via Google's SRE Book](https://sre.google/sre-book/availability-table/#appendix_table-of-nines)

`aggregate availability` = `successful requests / total requests`
If I have 2.5M requests per day and aim for 99.99% availability I could reasonably have 250 errors and still be within my target for that day. This can be done on a daily/weekly/monthly basis.

### KISS (Keep it simple, s....)
- Don't pick your targets based on the current system performance. Base it on what your end user ***actually*** needs.
- Don't create complicated aggregations for your SLIs. They need to be simple enough to reason about.
- No absolutes. You are not attempting perfection for any of your objectives. You want performance and happiness for your user.
- The less SLOs the better. Be clear on your objectives so you can actually meet them.
- Embrace change. SLOs can and will change as your system evolves.

### Control Measures
- Monitor your systems SLIs. 
- Compare SLIs to SLOs and decide whether action is needed.
- If action is required, you must determine ***what*** should be changed in order to meet your targets.
- Take the action.

### Conservative Promises or Broken Promises 
- Your published SLOs and internal SLOs can be different. You can build tighter internal SLOs to ensure you don't breach the advertised ones.
- If your system is meeting expectations, you may not need to put as much effort into improvement. You can shift work to toil reduction and technical debt payoff.

## Alerting & Priorities
- Priority 1
     - Must be dealt with immediately
     - Pages an on call
     - Leads to event triage
     - Impacts SLO
- Priority 2
     - Can be dealt with next business day
     - Not customer facing usually or limited impact scope
     - Email or channel alert
- Priority 3
     - Event is purely informational, likely gathered in dashboards or passive email
     - Includes capacity planning related info

### Common Monitors & Alerts
- Target Error Rate ≥ SLO Threshold - choose a time window and alert when error rate exceeds threshold for that window.
     - If SLO is 99.9% for 30 days we alert when the rate in the window we deem is above .1%

## Communication and Collaboration for SREs
- Be open. Be available (within reason).
- The most successful SRE teams display the above qualities. 
     - They are open and excited to collaborate with development teams to improve the health and reliability of their systems. 
     - They have a clear vision for what they want to deliver and leave room in their road maps for the needs of development teams to seek their assistance. In a perfect world, these road maps are created in collaboration with development teams.

## A Visualization of the Above
[This medium article](https://ernesenorelus.medium.com/building-sre-from-scratch-485e23985bbd) gives a very similar, high level outline of the topics I spoke about above. 
It also provides this flowchart which I feel does a great job of demonstrating the many areas which SREs are responsible for and wraps up this post well.
![alt](/assets/sreflowchart.png)

## Sources
- [SRE vs DevOps (Atlassian)](https://www.atlassian.com/devops/frameworks/sre-vs-devops)
- [SRE vs DevOPS (IBM)](https://www.ibm.com/think/topics/devops-vs-sre)
- [SRE (Google)](https://cloud.google.com/sre?hl=en)
- [Service Level Objectives](https://sre.google/sre-book/service-level-objectives/)
- [Monitoring & Alerting](https://sre.google/sre-book/monitoring-distributed-systems/)
- [Practical Alerting](https://sre.google/sre-book/practical-alerting/)
- [Incident Management](https://sre.google/sre-book/managing-incidents/)
- [Postmortem Culture](https://sre.google/sre-book/postmortem-culture/)
- [On-Call](https://sre.google/workbook/on-call/)
- [Alerting on Significant Events](https://sre.google/workbook/alerting-on-slos/)