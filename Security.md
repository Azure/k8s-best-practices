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

## Overview on technical aspects of securing an Kubernetes environment

The following diagram shows an overview of the most relevant aspects of security.
![](/securing_a_cluster/securing_a_cluster.png)

1. Securing the api server authentication/ authorization
2. Securing ingress and egress traffic from and to your cluster
3. Securing images
4. Securing secrets and connection strings in your applications
5. Securing network isolation of pods and app resources
6. Securing pods and pod capabilities

These topics and additional aspects of security will be described in the following chapters. Please see this as a guide towards a secure design of your cluster. While we strongly recommend that you evaluate your security needs and capabilities in your environment this does not mean that you have to apply everything as it will incur costs and increase operational complexity.

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
    * Secure images and pod admission controller
    * Container sandboxes
    * Managing secrets and privileged information
* Links

Links
=================

    > Good documentation that should be references
