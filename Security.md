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
    │   ├── Securing endpoints for api server and cluster nodes
    │   ├── Securing serviceAccounts and secrets
    │   ├── Securing communication paths    
    │   ├── Monitoring and Auditing of security relevant events
    │   ├── Running benchmarks and tests to validate cluster setup   
    │   ├── Configuration best practices       
    ├── Ensuring authentication and authorization
    │   ├── Configuring RBAC
    │   ├── Service accounts
    │   ├── Automating setup/ maintenance tasks   
    ├── Securing workloads
    │   ├── Secure images and admission controller
    │   ├── Pod identities, security contexts and pod security policies
    │   ├── Network segmentation
    ├── Special topics
    │   ├── Private clusters?
    │   ├── Forced tunneling
    └── Links

## Separating environments

    > Concepts that can be applied to ensure security isolation for different workloads
    > Separating Subscriptions, Resource Groups, Azure RBAC, Service Accounts and Secrets

- [ ] :fire: Cluster vs Namespace isolation
- [ ] :fire: Azure service principals and MSI
- [ ] :cloud: Dedicated nodes / hyper-v isolation on Nodes

## Securing a cluster

    > Understanding the cluster attack surface
    > Securing Service Accounts and secrets
    > Securing and maintaining host vms
    > Monitoring and securing security events and logs

- [ ] :boom: Master Endpoint security in AKS / ACS-Engine
- [ ] :boom: Securing access to host vms
- [ ] :boom: Upgrading and mainting hosts, apparmor, linux capabilities filter, os security patching
- [ ] :fire: Evaluation of security benchbmarks like KubeBench / CSI
- [ ] :cloud: Security Impact of activating addons and dashboard
- [ ] :cloud: Encrypted service to service communication
- [ ] :cloud: Service Endpoints for PaaS Service lockdown


## Ensuring Authentication/ Authorization

    > Concepts that can be applied to configure and bootstrap authentication in azure
    > Understanding Azure AD setup and the risk impact on security
    > Minimizing the blast radius by applying least priviliges inside and outside the cluster

- [ ] :fire: Azure AD Service Accounts and Groups
- [ ] :fire: Maintaining Secrets

## Securing Workloads

    > Understanding the attack surface from container images and laying out Microsoft 1st party and ecosystem options
    > Defining Pod security and ensuring minimal attack surface and good security default on apps
    > Isolating ingoing and outgoing traffic and monitoring relevant behaviour

- [ ] :fire: Image scanning in azure container registry and third party products like Twistlock, Neuvektor and Aqua
- [ ] :cloud: Ensuring adimission controllers/ pod security policies, privileged pods, runasroot, volumes, fsGroups, hostports on AKS / ACS-Engine
- [ ] :cloud: Capabilities of filtering network traffic with policies, azure firewall or network appliances

## Links
    > Good documentation that should be references
