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

## First Principles:

* Apply least privileged access
* Segregation of responsibility
* Integrate security into DevOps
* Trust your sources
* Minimise attack surfaxce
* Apply security in a layered approach

Table of Contents
=================

  * [Setting up environments](./Security_setting_up_environments.md) -> [BulletPoints](#setting-up-environments)
     * Separating environments
     * Setting up/ Validating virtual network
     * Provisioning clusters 
  * [Securing a cluster](./Security_securing_a_cluster.md) -> [BulletPoints](#securing-a-cluster)
     * Securing endpoints for api server and cluster nodes
     * Ensuring authentication and authorization
     * Setting up & keeping least privileged access for common tasks
     * Create administrative boundaries(namespaces) between resources as sample
     * Securing communication paths between namespaces (and nodes)   
     * Continous Monitoring and Auditing of security relevant events
     * Running benchmarks and tests to validate cluster setup
     * Regular maintenance, security and cleanup tasks
     * Configuration best practices        
  *  [Securing workloads](./Security_securing_workloads.md) -> [BulletPoints](#securing-workloads) `Dennis`
     * DenyEscalatingExec, Pod identities, security contexts and pod security policies
     * Securing serviceAccounts and secrets
     * Network segmentation (Ingress/ Egress)
     * Secure images and admission controller
     * Container sandboxes
     * Managing secrets and privileged information
  * Links


Setting up environments
=================

    > Concepts that can be applied to ensure security isolation for different workloads
    > Separating Subscriptions, Resource Groups, Azure RBAC, Service Accounts and Secret
    
- [ ] :fire: Cluster vs Nodes vs Namespace isolation
- [ ] :fire: Azure service principals and MSI
- [ ] :cloud: Dedicated nodes / hyper-v isolation on Nodes
- [ ] :fire: Inbound/ Outbound traffic (Forced Tunneling)
- [ ] :fire: Setting up RBAC

Securing a cluster
=================

    > Understanding the cluster attack surface
    > Concepts that can be applied to configure and bootstrap authentication in azure
    > Minimizing the blast radius by applying least priviliges inside and outside the cluster
    > Securing and maintaining host vms
    > Monitoring and securing security events and logs

- [ ] :boom: Master Endpoint security in AKS / ACS-Engine
- [ ] :boom: Securing access to host vms
- [ ] :boom: Configure RBAC
- [ ] :boom: Continous security using tools like Aqua, NeuVektor, Twistlock, SysDig
- [ ] :boom: Configure "dev" namespaces with permissions, rolebindings resource quotas and users
- [ ] :boom: Upgrading and mainting hosts, apparmor, linux capabilities filter, os security patching
- [ ] :fire: Evaluation of security benchbmarks like KubeBench / CSI
- [ ] :cloud: Maintenance of certificate and key rotation, cleanup of docker registry
- [ ] :cloud: Security Impact of activating addons and dashboard
- [ ] :cloud: Encrypted service to service communication across nodes
- [ ] :cloud: Service Endpoints for PaaS Service lockdown

Securing Workloads
=================

    > Understanding the attack surface from container images and laying out Microsoft 1st party and ecosystem options
    > Defining Pod security and ensuring minimal attack surface and good security default on apps
    > Isolating ingoing and outgoing traffic and monitoring relevant behaviour

- [ ] :fire: Image scanning in azure container registry, ValidatingAdmissionWebhook and third party products like Twistlock, Neuvektor and Aqua
- [ ] :cloud: Ensuring DenyEscalatingExec adimission controllers/ pod security policies, privileged pods, runasroot, volumes, fsGroups, hostports on AKS / ACS-Engine
- [ ] :cloud: Capabilities of filtering network traffic with policies, azure firewall or network appliances
- [ ] :cloud: Container sandboxes, gVisor, kataContainers
- [ ] :fire: Maintaining secrets in HashiCorpVaul, Azure KeyVault, Azure KMS Plugin

Links
=================

    > Good documentation that should be references
