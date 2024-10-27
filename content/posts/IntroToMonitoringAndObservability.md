+++
title = 'Monitoring & Observability, An Introduction'
date = 2024-10-27
draft = false
+++
## What ***is*** Monitoring?
The action you take. We monitor a system, an application, a metric to find anomalies that may indicate a problem. Monitoring allows us to determine whether our system is working correctly and exhibiting expected behaviors.

### Types of Monitoring
- Availability Monitoring & Site Reliability Monitoring - Most often associated with uptime. I touch on these concepts in this [post](https://notsuoholi.github.io/posts/srecore/#monitoring--alerting).
- [Application Performance Monitoring](#application-performance-monitoring)
- [RUM and Synthetic Monitoring](#synthetic-and-real-user-monitoring)
- Security Monitoring & Network Monitoring - Threat detection and security event management. I won't go into much detail on that for this series but you can begin to read more [here](https://www.splunk.com/en_us/blog/learn/security-monitoring.html).

### Monitoring Tools
- Observational - These can be hardware, software, and/or services which report on their operational status.
- Analysis - We use these to determine where and why problems are occurring in our system.
- There are several tools which allow us to accomplish the above such as Prometheus, Grafana, Tempo, Datadog,  Elasticsearch, Logstash, and Kibana, etc.

### Monitoring Challenges
The following challenges are often associated with traditional monitoring.
- Data gaps can limit the full visibility for a system or application.
- Slow movement and lack of modernization - Traditional monitoring tools don't always move as fast as the applications which they monitor when it comes to collection which can be a problem when we want to analyze our data in near real time.
- Too many tools! There are so many tools that can be used to collect and analyze data, it can be hard to find use for them all.

## What ***is*** Observability?
This is a property more than an action. It's how we understand the complex system we operate. Helps us answer the question of what is happening in our system and each of subsection of that system.

For a system to be considered as observable it must have data to observe (and lots of it, think MELT) as well as tools available to aggregate and operate on that data.

### Tools for Observability
Instrumentation Tools like OTEL help us collect telemetry data from various sources (applications/services) within our systems.
- Tools like this enable us to process and correlated data in our system which added context and visualization.

### Benefits of Observability
- Incident Response & RCA - Observability enables us to perform root cause analysis with greater efficiency thus cutting down on our incident response time. This allows us to reduce our MTTR (mean time to resolution) and improve the overall resilience of our system.
- An observable architecture directly supports enhanced application security and reliability efforts.

### Application Performance Monitoring
Unlike infrastructure monitoring, which focuses on resources like CPU, memory, and disk utilization, APM zeroes in on application performance.

It tracks metrics such as request rates, response times, error rates, and throughput, providing a clear picture of how an application serves user requests and handles backend dependencies.

- Real-Time Observability
    - APM tools allow us to monitor live transactions and provide us immediate insights into latency, bottlenecks, and application errors.
- Distributed Tracing & MELT Integration
    - With APM in place we are able to realize the benefit of distributed tracing which gives us a much clearer picture of how requests flow across the system.
    - Metrics: APM collects application-specific metrics (e.g., response times, error rates) and contributes to the broader metrics dataset.
    - Events: It captures significant events like transaction failures, anomalies, or SLA breaches.
    - Logs: APM tools often integrate with logging systems to correlate errors or performance issues with specific log entries.
    - Traces: APM specializes in tracing, capturing the sequence of application calls and dependencies, highlighting where issues arise.
    - [More on Telemetry and MELT](IntroToTelemetry)

### Synthetic and Real User Monitoring
#### Synthetic Monitoring
Involves deploying scripts to simulate the path an end user might take through a web application, reporting back the performance the simulator experiences. The traffic measured is not of your actual users, but rather synthetically generated traffic collecting data on page performance.

It is especially beneficial as a component of regression testing and application monitoring. It allows us to test our applications at every development stage and regularly in our production environment. If find that a change we made to the application causes degradation in performance this is the stage at which that would become clear. 

In short, synthetic monitoring can provide us additional insight into changes in our system ***before*** they negatively user experience.

#### Real-user monitoring (RUM)
Measures actual user experience. Usually, a third party RUM agent in JS is injected on each page of an application which then reports on page load data for each request made. It allows us to measure real user interactions.
- Datadog, Splunk, NewRelic, and Sentry are just some of the companies which offer RUM tools.

A combination of the two helps teams see how changes impact end users and monitor load times, interactivity, and errors from a user’s perspective.

### [Next Article...(Defining Reliability for Your System)](DefiningReliability)

## Sources
[APM](https://www.splunk.com/en_us/blog/learn/apm-application-performance-monitoring.html)
[RUM vs Synthetic Monitoring](https://www.splunk.com/en_us/blog/learn/apm-application-performance-monitoring.html)
[Monitoring vs Observability vs Telemetry](https://www.splunk.com/en_us/blog/learn/observability-vs-monitoring-vs-telemetry.html)
[Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/)
[SRE Core Principles](https://notsuoholi.github.io/posts/srecore/#monitoring--alerting)