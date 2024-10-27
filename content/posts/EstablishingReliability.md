+++
title = 'Establishing Reliability, a High Level Overview'
date = 2024-10-27
draft = false
+++
This is the first in a collection of articles which work together as a guideline for establishing Reliability and Observability within a system. Below are links to the other articles in this series.

1. Establishing Reliability (this article)
2. Intro to Telemetry
    - Intro to OpenTelemetry
3. Intro to Monitoring and Observability
4. Defining Reliability for Your System

**Note:** Below, I refer to applications, services, and the system as a whole. In a microservice-based architecture, all the individual services work together to create the overall application that an end-user interacts with. This context should help clarify some of the explanations that follow.

## Questions to Ask
- Are all services within the system observable?
    - Are we using a well-known framework or standard? (i.e. OpenTelemetry)
- How do we configure our Infrastructure?
    - IaC or manual config?
    - Can we identify any areas we may want to measure based on configuration?
- Have all previous outage causes or potential issues been identified and fixed?
    - If these above have not been fixed are they properly documented and work tracked?
    - Is there tech debt to consider?

## Understand Your Needs
If you are building reliability and observability into a system, it is most important to lean on the SMEs from your development teams and platform engineering teams. They are the people who understand the system you are to measure. 

Together, you must answer the questions laid out [above](#questions-to-ask).

## Select Tooling
Once you have a better understanding of the questions you need to answer, you can determine the tool which will best help you answer them. Select the tool which fits your needs best and allows you to unify data from several sources.

## Scheduling & SLAs -> SLOs -> SLIs
Work with each team on a rotating schedule to continue to develop an understanding of each piece of the system and its needs.

- Determine your SLAs, SLOs, and SLIs based on the information you can now gather from your system. It is worth asking yourself the following questions:
    - Do we have a solid understanding of our customer's needs? 
    - What would need to be present, at minimum, for our customers consider our system performant?
        - Do we have observability built into each of those parts?

- This time should also be used to educate teams on what behaviors their part of the application exhibits with the telemetry data currently being collected and where potential gaps lie.

## Monitoring - Establishing a Baseline
### MELT - Setup Telemetry & Monitoring for Priority 1 and Beyond
- MELT (metrics, events, logs, traces) must be in place for the services deemed priority one for our system.
    - Install collection agents and operator/APM agents in all relevant environments.
- Repeat this step for all services which your P1 service depends on or interacts with.
    - There may be parts of your system which rely on services you do not have control over or cannot instrument. Try to limit how often this is the case and try to remove single points of failure when it is.
### Automation
- Automate, automate, automate
    - As you move through the system, set patterns and automate repeatable processes where you can.
        - Identify common monitors and metrics you want to surface for all your services and create templates rather than starting from scratch each time you onboard new services.
    - What you build should be simple, understandable, and reusable.

## Perform Production Readiness Reviews
I won't go into details on this as the Google SRE book gives what I (and most industry professionals) feel is a great introduction and example of PRRs and their implementation. This process should be tailored to the needs of the business and agreed to by the development teams.
[SRE Engagement Model and PRRs](https://sre.google/sre-book/evolving-sre-engagement-model/)

## Document and Plan
The development teams will be responsible for the documentation and monitoring of their services and therefore must be the authors of documentation and SLAs for them. 
- Work with them to produce these documents and generate reusable guidelines for managing them going forward.
- Agree on and implement a plan to improve observability as the system evolves and grows in complexity.

## Incident Management
After completing the above for all relevant, high priority services, you can start putting together processes for incident response. These processes should be automated whenever possible.

### [Next Article...(Intro to Telemetry)](../posts/IntroToTelemetry)