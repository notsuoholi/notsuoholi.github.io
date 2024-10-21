+++
title = 'Security and YOU (the SRE)'
date = 2024-10-21T02:01:54-05:00
draft = false
+++
Though it may seem as though security might not need to be at the forefront of an SRE's mind, the reality couldn't be more different.
- Downtime Reduction - If build security into our practices, we will deliver a product with very few vulnerabilities which in turn means less vulnerability to exploit, and therefore less downtime.
- Compliance Standards - In order to meet compliance standards like [SOC 2](https://www.getkisi.com/guides/soc-2-type-ii#soc-2-type-ii-compliance), our in built procedures regarding security must be strong.
- No Damage to Brand - If we are breached, our reputation will be damaged, thus degrading consumer trust in our product.

## Most Common Threats Relevant to SRE
- DDoS Attacks - Aims to drive an overwhelming amount of simulated traffic to a service of piece of a service in order to cause it to be unavailable for use by normal end users.
- Software exploits - If there is an issue or vulnerability in a specific version of our product or a service we use within our product this could be used by an attacker to gain access to our service or infrastructure.
- Phishing - Social engineering can be used to get employees to give up sensitive information and gain access to our service and infrastructure.
- Ransomware - Gaining access to a piece of our infrastructure or codebase as using this as a means to demand payment.

## Best Practices for Mitigating Threats
### Access Control
Controlling access to resources must be a priority. 
- Commit to the principle of least privilege and only give access to exactly what is necessary for a person or team to complete a task or project. No more, no less.
- Use Terraform to provision access to resources in your cloud infrastructure for individuals.
- Tightly control which authenticated users can access which resources.
- Prefer service accounts over user accounts and deploy infrastructure with tools such as Atlantis which reduces attack surface should a user account be exploited.
- Service accounts should have heavily restricted access. Typically, a service account should only have access to a limited number of roles which are also tightly controlled to reduce attack surface in the case of an intrusion.
- Understanding how Service accounts are used by attackers is important for understanding Cloud security.
- Impersonation
    - Exported account keys.
    - ActAs, JWT (JSON Web Token) in Cloud.
- Restrict Access to objects in cloud object storage from public and only allow restricted access to the contents inside.

### Monitor, Monitor, and Monitor
Well implemented monitoring systems are our first line of defense in preventing security breaches and also incredible resource when facilitating analysis of security breaches.

### Example Workflow for Security Monitoring
- Use Prometheus for collecting metrics for your applications and system. You can use data related to things like error rates or CPU spikes at unusual intervals to determine if there is something suspicious occurring.
- Use a log aggregator like Loki to collect and search your application logs to find things such as failed login attempts or logs from an IPS or IDS.
- Use something like Stackdriver (GCP) to view metrics for your ingresses and find unusual traffic patterns or access violations.
- Alert on the metrics from Prometheus and Stackdriver when patterns deviate too far from expectation.

### Disaster Recovery, Backups, & Response Playbooks
In the event that a security breach does happen, you must ensure you have recent backups and recovery plans in place. Test them regularly to make sure they behave as intended.

[AWS Incident Reponse Playbook Samples](https://github.com/aws-samples/aws-incident-response-playbooks/tree/master)

### Hardening Your Infrastructure
- Review key principles of hardening cloud infrastructure (e.g., least privilege, minimizing attack surfaces, encrypting data at rest and in transit, using IAM roles).
- Lateral movement and privilege escalation techniques.
    - Cloud Service Accounts can be used for lateral movement and privilege escalation in Cloud environments. Ensure you are operating on PoLP to prevent attack surface.

### Cloud and Cluster Security
- Regularly audit your system for any vulnerabilities with tools like Dependabot, Snyk, and SonarQube.
- Use trusted images and modules (i.e. Terraform) published by trusted, and verified authors.
- Do not run containers as root to minimize impact should container be breached.
- Set limits on resources in the even they are accessed so system resources are preserved for other, unaffected workloads.
- Only allow communication between pods on a necessary basis to limit attack surface. You can accomplish this by implementing a service mesh to manage traffic between services.
- Implement RBAC policies in a Kubernetes cluster which specify only the access required for any specific role to run and no more.
- Store secrets in a manager such as Google Secrets Manager or Hashicorp Vault and inject them at runtime. Do NOT store secrets in your codebase or Dockerfiles.
- Implement version pinning for your images. Run a version 1-3 releases behind the latest release to ensure you do not encounter application breaking changes, version incompatibility, or vulnerability exploits that might be found in a `:latest` release.
- Set pod security policies which are the base conditions pods must adhere to in order to run inside the cluster. i.e. A policy which restricts pods from being able to run as root set on pods with a specific label.

### Data Encryption & Methods
Asymmetric vs Symmetric
- Asymmetric (RSA) is slow, but good for establishing a trusted connection. Like connecting to a bastion using a public/private key pair.
- Symmetric (AES) has a shared key and is faster. Protocols often use asymmetric to transfer symmetric key. Perfect forward secrecy. Think WhatsApp or Signal. Can also be used for things like secure online shopping or quickly encrypting/decrypting file data.


### SIEM (Security Information and Event Management)
- Splunk is one of the most common SIEM tools available for monitoring logs and security data in real time to find vulnerabilities.
    - You can setup automated searches to detect anomalies, and trigger alerts on activity deemed as suspicious.
    - You can also visualize data in dashboards.

## Secure Networking
### Firewalls and Rules
- A firewall allows you to define rules for both inbound and outbound traffic. This puts an SRE in the perfect position to deny specific traffic based on source or destination IP addresses, ports, and protocols. This powerful tool is very helpful in preventing unauthorized access to our resources and infrastructure. 
- Most modern, cloud provided firewalls or FWaaS tools have built in protection from Intrusion. They also provide a method for mitigating DDoS attacks because they allow you to filter traffic that exceeds predefined, acceptable thresholds or appears malicious.

### Ingress
- An ingress is a networking component which exposes HTTP/HTTPS routes outside a k8s cluster to services internally. Traffic rules are defined and controlled in the ingress resource itself.
- You must have an ingress controller for an ingress to work in your cluster. Think of something like NGINX, Kong, or HA Proxy.
- It allows us to set rules for how you want traffic to flow in and out of our application(s).
- Benefits include:
    - Path-based and host-based routing to reduce number of entry-points we expose
    - TLS termination and encryption - The ingress handles SSL/TLS termination to establish secure HTTPS connections so we know our data is encrypted and safe from inspection or interception.
    - We can implement rate-limiting, IP white-listing to further control access to our services.
    - These work in tandem with web-application firewalls to protect against attacks and export metrics and logs we can use to detect anomalies and threats.

### Layer 4 Load Balancer
- Operates at the transport layer (OSI Layer 4), distributing traffic based on IP addresses, ports, and protocol information, without looking into the actual data of the packets.
- Provides fast, efficient routing for TCP/UDP traffic but lacks the ability to make complex routing decisions based on application-level information.

### Layer 7 Load Balancer
- Works at the application layer (OSI Layer 7), inspecting HTTP headers, URLs, and cookies to make intelligent routing decisions based on application-specific data.
- Enables advanced features like content-based routing, SSL termination, and traffic manipulation for better performance, security, and customization.

### The Power of the Three
When all three work together, we achieve a layered, secure approach to application delivery.

- The LB handles traffic distribution and acts as the first line of defense before traffic reaches the application layer. 
- The ingress controller operates as described about and determines how requests are handled and directed to services based on the rules we set in our configuration and it enhances the security of our application via SSL.
- Finally, our WAF offers an additional layer of security by enabling us to inspect incoming traffic for potential threats. We can filter and monitor HTTP requests and stop unusual requests or potential attacks before they have the chance to cause damage.

| **Component**               | **Function**                                   | **Flow**                     |
|-----------------------------|------------------------------------------------|------------------------------|
| **Client**                  | Sends HTTP/HTTPS requests                      | ↓                            |
| **Load Balancer**           | Distributes incoming traffic                   | ↓                            |
| **Ingress Controller**      | Manages routing based on defined rules        | ↓                            |
| **Web Application Firewall** | Inspects and filters requests for threats     | ↓                            |
| **Backend Services**        | Receives clean, routed traffic                 |                              |


### The Power of HTTP Headers & CORS
- CORS
    - Cross-Origin Resource Sharing. Can specify allowed origins in HTTP headers. Sends a preflight request with options set asking if the server approves, and if the server approves, then the actual request is sent (eg. should client send auth cookies).
- Headers:
    - X-Content-Type-Options - prevents browsers from interpreting files as different types than intended. Gives us some protection against attack.
    - Content Security Policy - Specifies which sources can load content on site/application. Can be used to prevent malicious scripts from being run.
    - Strict-Transport-Security (HSTS) - Ensures browsers can only connect to site using HTTPS. Results in more secure connections to application and prevents "eavesdropping".

## Certificates & Management
Certificates enable us to encrypt data in transit and protect against the exposure of sensitive data and man-in-the-middle attacks. 
- They also enable more trust in our users as they are provided the guarantee of their data being encrypted and that there is established trust between the client and server they are interacting with.
- They enable us to meet compliance standards like [GDPR](https://gdpr.eu/).

### Tools for Cert Management
- Cert-Manager - Open-source add-on for Kubernetes which automates management and issuance of TLS certs from various sources for the cluster.
- Let's Encrypt - Free and automated cert authority which provides SSL/TLS certs. It allows one to obtain certs quickly and renew them automatically which ensures secure HTTPS connections to public addresses.
- OpenSSL - Used for generating SSL/TLS certs. Can be self-signed as well as signed by an authority. This gives it the flexibility to be used for creating certs for internal services or test environments.

## Sources:
- [Security in SRE](https://medium.com/wanna-know-whats-next/security-with-sre-7891c4af24fd)
- [Security Engineering Notes](https://github.com/gracenolan/Notes/blob/master/interview-study-notes-for-security-engineering.md#networking)
- [AWS Incident Response](https://github.com/aws-samples/aws-incident-response-playbooks/blob/master/playbooks/IRP-DoS.md)