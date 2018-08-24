# Guidelines for Securing Kubernetes on Azure

This is intended as a guide on how to secure a production ready Kubernetes environment running on Azure.

## What is this

When it comes to securing your Kubernetes production environment in an enterprise, there are many tutorials and opinions that exist in the community.
We strive to collect well founded concepts and help the reader to evaluate options and balance tradeoffs.

Feedback welcome!

To the editors:
    > See this as mvp - more detailed topics will come once we have covered the basics
    > We should focus on parts that are azure specific on which we will contribute content.
    > Please provide references to good Kubernetes resources.

The severity or importance of each topic is indicated by an emoji in the topic name.

* :boom: Critical
* :fire: High
* :cloud: Medium
* :partly_sunny: Low

## Security design principles:

* Minimise attack surface
* Defense in depth
* Establish secure defaults
* Apply least privileged access
* Segregation of responsibility
* Integrate security in every process
* Keep security simple

Table of Contents
=================

* [Setting up environments](./Security_setting_up_environments.md)
    * Separating environments
    * Setting up/ Validating virtual network
    * Provisioning clusters 
* [Securing a cluster](./Security_securing_a_cluster.md)
    * Securing endpoints for api server and cluster nodes
    * Ensuring authentication and authorization
    * Setting up & keeping least privileged access for common tasks
    * Create administrative boundaries(namespaces) between resources as sample
    * Securing communication paths between namespaces (and nodes)   
    * Continous Monitoring and Auditing of security relevant events
    * Running benchmarks and tests to validate cluster setup
    * Regular maintenance, security and cleanup tasks
    * Configuration best practices        
*  [Securing workloads](./Security_securing_workloads.md)
    * DenyEscalatingExec, Pod identities, security contexts and pod security policies
    * Securing serviceAccounts and secrets
    * Network segmentation (Ingress/ Egress)
    * Secure images and admission controller
    * Container sandboxes
    * Managing secrets and privileged information
* Links

Links
=================

    > Good documentation that should be references
