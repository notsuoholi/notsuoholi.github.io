+++
title = 'Defining Reliability within Your System'
date = 2024-10-27
draft = false
+++
This outline provides an approach to building reliability into your system or application. To fully grasp the concepts discussed here, it’s essential to understand the concepts discussed in the other articles in this collection, as they serve as foundational building blocks.

## Understanding Business Requirements and User Expectation
Before diving deep into the actual system, we must gain a deeper understanding of what is expected of our application.

### Build what you need not what you think you need
- Speak with the development teams, platform engineers, and business stakeholders to understand our critical business objectives. 
    - Determine what our users expect, at minimum, from our application. Don't focus on anything that isn't identified as important to the business or user's of the application.
- Identify critical user journeys. 
    - Map out primary functions (user journeys) that directly impact customer satisfaction and business objectives. This is application specific.
- Clarify our objectives and Key Performance Indicators. 
    - Are we aiming for high availability? What exactly does that look like? 
    - Setting these goals helps inform our SLO benchmarks.

## Defining What's Important
In order for development teams to identify and define SLOs and SLIs, they (and you) must understand what they are. Below is a short introduction.

### SLI -> SLO -> SLA?
- Service Level Indicators (SLIs) are specific, measurable characteristics of the service, such as latency or error rates.
- Service Level Objectives (SLOs) are the target values for SLIs our service aims to meet.
- Service Level Agreements (SLAs) are contractual agreements with customers that include consequences for failure (usually a financial penalty or reputation damage).
- Error budgets are the amount of room we have to accept errors before breaching our SLAs. Google's [Availability table](https://sre.google/sre-book/availability-table/) is a great example of leaving room for error.

More on SLIs, SLOs, and SLAs can be read [here](https://notsuoholi.github.io/posts/srecore/#sli---slo---sla--error-budgets).

### The Four Golden Signals 
Latency - The time it takes to serve a request. Think of something like request duration for a given request.
Traffic - A measure of how much demand is being placed on your system. How many requests are we processing for a given service per second?
Errors - The rate of requests that fail explicitly, like a 5xx, or implicitly, like a 2xx coupled with the wrong result.
Saturation - How ***full*** a service is or its overall capacity. How many resources are being constrained on the service at a given time. Latency increases are often a marker for saturation of a service.

### Prioritize!
Ensure you are choosing metrics and signaling on data which matters most to the application's success. This should be based on the primary functions (user journeys) we defined in previous steps.

### Setting Initial SLOs
The goal for setting your SLOs is not perfection out of the gate. Aim for targets you can actually hit and iterate from there. 

Set your initial thresholds based on what you ***expect*** your traffic to look like, load test data can help here. Compare your application to others of the same type, if there are any, and set similar benchmarks.

Utilize error budgets. Use them to give yourself guardrails and also the flexibility to evolve and improve with time. More on error budgets [here](https://notsuoholi.github.io/posts/srecore/#error-budgets).

A great article on setting SLOs from scratch can be found [here](https://medium.com/@asuffield/defining-an-slo-6302f60b218a).

## Feedback and Iteration
After setting proposed SLOs, test them against any and all data (metrics) available. This will likely be a combination of real and synthetic.

After testing, review the SLOs once again with development teams. It's important to ensure they are aligned with current capabilities of the infrastructure and teams supporting your application.

## Monitoring and Alerting
Once you've agreed to implement V1 of your SLOs, you can define and implement SLIs which track against them in order to alert when we're at risk of breaching them.

Take time when crafting your SLIs. There is a real risk of creating too much noise in some cases or failing to catch errors because of poorly designed measures.

## Documentation and Communication
### Create Clear Documentation
Write documentation outlining the SLOs selected by SRE and development teams, the reasoning behind them, and their impact on the application.

### Set Communication Protocols
Define how and when the SRE and/or development teams will communicate breaches or issues and what steps to take if an error budget is exhausted.

## Refine and refine again. 
At the outset, your SLOs probably won't be perfect. That is okay and expected.

Once your application is live, monitor your SLOs closely and gather data on their performance. 

You will need to adjust your SLOs based on real-world performance, user feedback, and the needs of the business.

The goal of the SRE and development teams should be to steadily improve application performance until you can meet your SLOs consistently and continue to fine tune them as the application improves.

## SLI -> SLO - An Example:
Let's say you are attempting to setup monitoring for an e-commerce application, a simple, typical example. Below are the steps you could follow to establish your initial SLO targets and SLIs which feed into those targets.

### Define SLOs Based on User Expectations
Set your SLOs based on user journeys and business needs:
- **Availability**: 99.9% uptime for key endpoints over 30 days.
- **Latency**: 95% of requests to `/checkout` and `/product` should respond within 100ms.
- **Error Rate**: Less than 0.1% error rate for all API requests.

### Identify SLIs to Measure SLOs
Once you've defined your SLOs, select SLIs that will measure those goals accurately:
- **Availability**: Track the percentage of HTTP 200 responses for critical endpoints.
- **Latency**: Measure response times for `/checkout` and `/product`.
- **Error Rate**: Monitor 5XX errors across all requests.

### Set SLI Targets to Match SLOs
Set SLI targets aligned with your SLOs:
- **Availability SLI Target**: 99.9% uptime for critical endpoints.
- **Latency SLI Target**: 95% of `/checkout` and `/product` requests under 100ms.
- **Error Rate SLI Target**: Less than 0.1% 5xx errors.

By defining SLOs first, you focus on user-centered goals, then measure these directly with SLIs. 

#### On SLAs
Sometimes it is the case that a service or application must adhere to specific SLA's which is often the case of financial applications. In this scenario, it is wise to think about SLAs and how they inform your SLOs.

### Final Word
I hope this series has offered you a clearer understanding of the components that build observability in a system. Feel free to connect with me via LinkedIn or GitHub (links below) if you'd like to contribute, or if you've noticed any errors or missing information.