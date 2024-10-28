+++
title = 'Telemetry, An Introduction'
date = 2024-10-27
draft = false
+++
## What ***is*** Telemetry?
The process of collecting and analyzing data from various sources to gain insights into our system's overall performance. It's critical to our decision making process and helps us with monitoring and measuring the observability of our system.

### Telemetry vs Monitoring
- Monitoring has a much narrower focus. The main objective one has for monitoring a system is to detect for potential issues and determine which actions to take to avoid or resolve incidents and escalations. Typical measures include metrics for resource usage and networking activity.

- Telemetry collects and analyzes data for a broader range of purposes. Ranging from troubleshooting to understanding user behaviors and system performance. It also uses a wider subset of metrics to gather and analyze said data.

Monitoring is effectively a subset of telemetry.

### Telemetry Types
There are many type of telemetry data one can measure an any IT System. These include things such as error rates, response times (latency), CPU and memory usage, disk I/O, and network throughput.

- User Telemetry Data
    - We can collect data about the users who engage with our product by collecting data on certain clicks, logins, page views, or page errors.

- Network Telemetry Data
    - We can collect data on bandwidth, networking ports, devices, uptime for devices used, etc.

- Application Infrastructure Telemetry Data
    - Typically, we include latency data, transaction and/or query volume (per second/minute format), database access, application error volume, deployment errors, etc.
    - We might also be interested in understanding who is interacting with our application and in which ways. 
        - Data around IP addresses, regions, OS types, and browser type/version give us insight into these areas.

### Gathering Telemetry Data (MELT)
Metrics - A numeric value measured over an interval of time. Includes attributes such as timestamp, event name, and a value related to that event.
- These are structured by default and easier to query.
- Because of their small size, they optimal for long term storage and can be used to understand a system's historical trends.
- They are best used for large data sets or data collected at regular intervals which aim to answer specific questions.
    - i.e. - How much CPU is utilized for a given service during a specific window of time? How many errors occurred during a peak traffic window for a given service?

Events - A record of a **significant** occurrence or change within a system/application. 
- They represent distinct points in time which capture specific system activities, changes in state, or user actions.
    - They focus on particular moments or actions like service deployments, errors, user sign-ins, HTTP requests/responses, etc.
- They give us context for the metric data we gather at any given time.

Logs - A text record of an event that happened at a particular point in time. They're considered the source of truth for what an application (and its underlying services) is ***actually*** doing. They come in three formats:

Plain text - the most common type, simplistic format
```
2024-10-26 12:00:00 INFO User login successful: user_id=12345
```
Structured - Becoming more standard as they are more easily queried. Usually follow a JSON or key-value format.
```
{
"timestamp": "2024-10-26T12:00:00Z",
"level": "INFO",
"event": "UserLogin",
"user_id": 12345,
"ip_address": "192.168.1.100",
"message": "User login successful"
}
```
Unstructured - These do not follow a standardized format. They are less common and more difficult to parse but more human-readable.
```
User login attempt from IP 192.168.1.100 at 2024-10-26 12:00:00: success for user 12345
```

Traces - A representation of the end-to-end journey of a request through a distributed system. Because many operations are performed for any single request, each request is encoded with important data (a span) which includes the trace ID, span ID, operation name, start/end timestamp, logs, events, and other indexed info.
- Traces are incredibly powerful in that they allow us to identify, for any given request, which part of your application ran into errors or degraded performance. 
- They are integral to diagnosing and resolving problems in distributed systems.

### Using Telemetry
Data we gather through telemetry can help us answer many questions and improve our system.

- Feature Development
    - Telemetry data gives us clearer insight into the least and most used features of our products by end users. 
    - This information helps us determine which parts of our application to focus on for improvements and enhancements and which we might want reduce in size or drop entirely.

- Identifying Problems
    - This is one of the most common uses in telemetry data and is usually associated with monitoring.
    - If we can reveal problems within our application like slowdowns and errors before they become serious problems for our users the we are using this data to the best of our abilities.

- Performance Optimization
    - Identifying bottlenecks and performance issues is another key use of telemetry data.
    - Which pages are slow to load, which functions which trigger queries to our DBs are slow to run? Identifying these issues will give us clear areas to focus improvement efforts.

- Change Validation
    - Part of developing and maintaining a product is continuously enhancing and improving it. 
    - We can use telemetry data validate whether we are improving our application with changes or introducing additional complexity with no benefit.

- Security
    - Telemetry data plays a vital role in system security. If we have insight into expected usage patterns, we can alert on anomalies in that data to call out potential threats, intrusions, and abuse.

### Telemetry Challenges
While telemetry data is essential for understanding and improving our application, it is not without challenges.

- Data Privacy
    - Collecting information such as IP addresses may raise privacy concerns as this and other sensitive user information should be handled according to whichever data privacy regulations and guidelines your specific application must adhere to. 
- Volume of Data & Analysis
    - Telemetry generates a host of data and as your system grows, so does the amount data you collect and measure. 
    - You must ensure you are able to scale and handle increased data volume in a reliable and cost effective manner.
    - Analyzing this large volume of data also produces significant challenge. The tools you use to analyze the data you collect must work for your purpose and scale for you to gain meaningful insight from the data collected.
- Data Integrity and Interoperability
    - Because telemetry data can be collected from a variety of interconnected systems, it is important to ensure the data collected remains as consistent as possible. 
    - Sanitize the data you collect by removing incomplete data and inaccurate data. If you've instrumented an observability framework such as OpenTelemetry, a lot of this is  already built in.

### [Next Article...(Intro to OpenTelemetry)](IntroToOpenTelemetry)

## Sources:
[Telemetry 101](https://www.splunk.com/en_us/blog/learn/what-is-telemetry.html)
[What is OTEL](https://www.splunk.com/en_us/blog/learn/opentelemetry.html)
[Monitoring vs Telemetry vs Observability](https://www.splunk.com/en_us/blog/learn/observability-vs-monitoring-vs-telemetry.html)
[MELT](https://www.splunk.com/en_us/blog/learn/melt-metrics-events-logs-traces.html)