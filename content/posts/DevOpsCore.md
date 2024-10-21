+++
title = 'DevOps & Release Management - Core Principles'
date = 2024-10-10T22:20:49-05:00
draft = false
+++
The below serves to outline some of the key aspects of DevOps and Release Engineering. It is not at all a complete guide but does cover some of the most important components. I will add to is as I continue to dive deeper.

## DevOps - A delicate balance of development and operations.

- A focus on continuous delivery and automation in the software development lifecycle. 
- Champion guidelines, practices, and culture that aims  to break down the silos which traditionally separate development, operations, networking, and security.

- Responsible for delivering the features that improve customer experience alongside developer teams. They keep development cycle moving quickly and efficiently.

- Ensure there are strategies in place for deploying new releases without causing service interrupts and proper rollback strategies in place for features that do cause issues.

- Focus Areas:
    - DevOps practices and tools (e.g., CI/CD pipelines, infrastructure as code).
    - Tools: Docker, Kubernetes, Terraform, AWS, GitOps.
    - Cross-collaboration skills (working closely with developers).

## Four Pillars of DevOps
- Communication
    - Constant communication between development and operations teams between planning and release phases.
    - This allows impediments to be resolved before they become blockers and improves the overall quality of the release process.

- Collaboration
    - Collaboration between development and operations teams help streamline processes.
    - When both are in sync, target infrastructure will be well prepared for incoming changes.
    - Helps avoid delays in delivery when well aligned.

- Automation
    - Build processes that are predictable, well-tested, and repeatable allow for stable environments and steady releases.
    - Tools like Chef and Puppet for configuration management can be used for this purpose.

- Monitoring
    - Monitoring the process as it has been, as it presently is, and as it could be are all important for improving the overall release process and should be measure regularly to provide inputs for possible improvements.
## Four Principles of Release Engineering

### Self-Service Model & Mindset
Teams must be self-sufficient to operate at scale. 
- Develop a set of best practices and setup tooling which allows for development teams to control and run their own release processes.
    - With a highly developed self-service model in place, there is only need for human intervention when problems arise.

### Moving at a High Velocity
- Build fast and deploy frequently.
    - Setup CI/CD pipelines which allow teams to consistently update their codebase and deliver regular, incremental updates to the product on schedules they determine. 
    - There are several options for CI like Git as well as CD like CircleCI or Github Actions.
    - Ensure you have built testing into not only your applications but into your pipelines as well.

### Hermetic Builds
- Build tools must ensure consistency and repeatability. It should not matter where you built the new feature (insensitive to libraries and other software on the build machine).
- Build are only dependent on known versions of our build tools like compilers and dependencies like libraries.
- We can **cherry pick** to fix bugs in older releases by rebuilding them at the same revision as that original build and only include specific changes submitted after.

### Enforcement of Policies & Procedures
Ensure there are security and access control procedures in place which determine who can perform specific operations when running a release.
- ***Gated Operations***
    - Approving code changes via config files throughout the codebase. Might include something like a CODEOWNERS file which is used for final approvals.
    - Specifying actions to be performed during release
    - Creating the release
    - Approving the proposal for integrating the release
    - Deploying the release
    - Making changes to build configurations for a project

## Build Tools and Config Management

### Mutable vs Immutable Infrastructure
Infrastructure you build will always be open to change. 
For example, a VM running a specific image or application. If you were only able to make changes to it by completely destroying and recreating it, it would be considered immutable. If you were able to make updates to it, it is considered mutable.

Tools like Chef, Puppet, and Ansible focus on changes to mutable infrastructure whereas tools such as Terraform and Packer focus on the provisioning and configuration of fresh infrastructure and not focused on change after creation.

When dealing with mutable infra, it is important to consider drift as infrastructure evolves and changes. With every update, there is more drift in state. With immutable infrastructure, this is not the case. 

### Declarative vs Procedural Build Tools
Declarative languages like Terraform, CloudFormation, Puppet, etc. focus on a declarative style of code which specifies the desired end state of your configuration. The tool used is the one to designate how that state will be achieved.

On the other hand, procedural style tools like Chef and Ansible allow one to write code in a step-by-step approach to achieving the desired state.

### Procedural Tooling Limitations
- They do not capture the current state of infrastructure. Without engineering a solution for capturing state (possibly within another code block) there is no built in method for reading back state for your current infrastructure.
- Code re-usability is limited. The code you wrote one week before may no longer be relevant for the resource your targeting today.

- Best used in situations wherein:
    - Infrastructure is simple
    - State is unimportant (test resources) and orphaned resources cleaned up regularly


## Sources:
- [Release Engineering](https://sre.google/sre-book/release-engineering/)
- [How SRE Relates to DevOps](https://sre.google/workbook/how-sre-relates/)
- [Effective DevOps](https://www.oreilly.com/library/view/effective-devops/9781491926291/)
- [Configuration Management Tools](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#b264)