# Guidelines for Securing Kubernetes on Azure

This is intended as a guide on how to secure a production ready Kubernetes environment running on Azure.

## What is this

When it comes to securing your Kubernetes production environment in an enterprise, there are many tutorials and opinions that exist in the community.
We strive to collect well founded concepts and help the reader to evaluate options and balance tradeoffs.

Feedback welcome!

    > To the editors: See this as a base line - comment on todos, feel free to add clearification on the structure and collect good source references for guidlines. 
    > We should focus on parts that are azure specific on which we will contribute content and reference good content for Kubernetes specific topics.
    > We will start the actual content once we have reviewed this and split the contribution of content.

The severity or importance of each topic is indicated by an emoji in the topic name.

* :boom: Critical
* :fire: High
* :cloud: Medium
* :partly_sunny: Low

## Structure

    .
    ├── Separating environments
    ├── Securing a cluster
    │   ├── Securing Endpoints
    │   ├── Securing ServiceAccounts and Secrets
    │   ├── Monitoring and Auditing of security relevant events
    │   ├── Running Benchmarks and tests to validate cluster setup   
    │   ├── Configuration best practices       
    ├── Ensuring Authentication/ Authorization
    │   ├── Configuring RBAC
    │   │   ├── Users (Developers/ Administrators)
    │   │   ├── Service Accounts
    │   ├── Service Accounts
    │   ├── Automating setup/ Maintenance tasks   
    ├── Securing Workloads
    │   ├── Secure Images and Admission Controller
    │   ├── Pod Identities, Security Contexts and Pod Security Policies
    │   ├── Network Segmentation
    ├── Special Topics
    │   ├── Private Clusters?
    └── Links

## Separating environments

    > Concepts that can be applied to ensure security isolation for different workloads
    > Separating Subscriptions, Resource Groups, Azure RBAC, Service Accounts and Secrets

- [ ] Cluster vs Namespace isolation
- [ ] Dedicated nodes / hyper-v isolation on Nodes
- [ ] Azure service principals and MSI

## Securing a cluster

    > Understanding the cluster attack surface
    > Securing Service Accounts and secrets
    > Monitoring and securing security events and logs

- [ ] Master Endpoint security in AKS / ACS-Engine
- [ ] Evaluation of security benchbmarks like KubeBench / CSI
- [ ] Security Impact of activating addons and dashboard


## Ensuring Authentication/ Authorization

    > Concepts that can be applied to configure and bootstrap authentication in azure
    > Understanding Azure AD setup and the risk impact on security
    > Minimizing the blast radius by applying least priviliges inside and outside the cluster

- [ ] Azure AD Service Accounts and Groups
- [ ] Maintaining Secrets

## Securing Workloads

    > Understanding the attack surface from container images and laying out Microsoft 1st party and ecosystem options
    > Defining Pod security and ensuring minimal attack surface and good security default on apps
    > Isolating ingoing and outgoing traffic and monitoring relevant behaviour

- [ ] Image scanning in azure container registry and third party products like Twistlock, Neuvektor and Aqua
- [ ] Ensuring adimission controllers on AKS / ACS-Engine
- [ ] Capabilities of filtering network traffic with policies, azure firewall or network appliances

## Links
    > Good documentation that should be references
