# Guidelines for Operational Excellence - Kubernetes on Azure

This is intended as a guide on how to operate a production ready Kubernetes environment running on Azure.

## What is this

When it comes to operating your Kubernetes production environment in an enterprise, there are many tutorials and opinions that exist in the community.
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

## Operational principles:

* TBD

Table of Contents
=================

* Deployment
    * Infrastructure as Code
        * Azure Resource Manager templates
        * Terraform
    * Helm charts
    * [Choose the right VM size for your agent nodes](./Cost_Optimization.md)
* Maintenance
    * Patch management
    * Upgrade management
* Monitoring
    * AKS cluster
    * AKS master components
    * acs-engine
* Identity and permissions
    * Azure RBAC roles
    * Kubernetes RBAC
    * Azure Active Directory integration
* Scaling
    * [Cluster Autoscaler](./Cost_Optimization.md)
    * [Horizontal Pod Autoscaler](./Cost_Optimization.md)
* Troubleshooting
    * Common issues
    * Kubelet logs
    * SSH node access
* Links

Links
=================

    > Good documentation that should be references