# Guidelines for cost optimization - Kubernetes on Azure

This is intended as a guide on how to optimize a production ready Kubernetes environment running on Azure.

## What is this

When it comes to optimizing your Kubernetes production environment in an enterprise, there are many tutorials and opinions that exist in the community.
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

## Cost optimization principles:

* Reducing OPEX
* Flexible on-demand capacity

Table of Contents
=================

* [Cluster Autoscaler](#cluster-autoscaler)
* [Horizontal Pod Autoscaler](#horizontal-pod-autoscaler)
* [Azure Container Instances connector](#azure-container-instances-connector)
* [Node - VM sizes](#node---vm-sizes)
* [Links](#links)

# Cluster Autoscaler

The cluster autoscaler (CA) is used for node scaling in an AKS cluster and can reduce the cost of the cluster significantly. Depending on your workloads you define a minimum number of nodes to keep the AKS cluster and your workloads operational, when the traffic is minimal to normal. Keeping the cluster from scaling its nodes to the limit of 100, you should also define a maximum number of nodes, when traffic increases.

The CA checks every 10 seconds for pending pods or empty nodes and scales the AKS cluster appropriately. Nodes are removed from the cluster, if they are unneeded for more than 10 minutes.

Normally this action takes place, when you deploy new workloads to your AKS cluster or removing workloads.

# Horizontal Pod Autoscaler

The horizontal pod autoscaler (HPA) provides your application with an autoscaling option based on metrics like CPU, memory or network utilization. It scales your application between the defined range of minimum pods, that keeps the application operational, and maximum pods to cover the increased load.

CA and HPA are working well together. So, the general recommendation is to enable both autoscaler options in your AKS cluster. Covering short peaks via the HPA to scale out only the application and longer peaks in conjunction with the CA provisioning additional nodes.

# Azure Container Instances connector

Another option for scaling your applications is the usage of the Azure Container Instances connector. The ACI connector comes in as a Virtual Kubelet in your AKS cluster represented by a virtual agent node with nearly unlimited capacity depending on your Azure subscription limits.

It is not a replacement for the HPA, but can be used as a replacement for the CA. When the CA is adding nodes to the AKS cluster, it takes time until the nodes are provisioned and ready for pod deployments. On ACI new pods can be spin up instantly.

Since Microsoft Ignite 2018 in September it is possible to deploy Azure Container Instances into an existing VNET subnet. The ACI connector capability for that is currently available as a private preview called AKS virtual node. Further information about ACI VNET integration and private preview sign up can be found under the following links.

* [Deploy container instances into an Azure virtual network](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-vnet)
* [AKS virtual node preview sign up](https://aka.ms/aks-virtual-node-preview-sign-up)

# Node - VM sizes

Selecting the correct VM size can have a significant impact on the operational cost of an AKS cluster. The general recommendation for production clusters are the VM series Dsv3 and Esv3 that are SSD backed. Most workloads can be covered by the VM sizes Standard_D2s_v3 with 2 vCPUs and 8 GB memory or Standard_D4s_v3 with 4 vCPUs and 16 GB memory. If you need a higher vCPU to memory ratio, then go with Standard_E2s_v3 with 2 vCPUs and 16 GB memory or Standard_E4s_v3 with 4 vCPUs and 32 GB memory.

For special requirements like GPUs you can go with different VM sizes out of the N-series.

AKS clusters for dev/test can be operated with the VM sizes Standard_B2ms, Standard_B4ms or Standard_B8ms to reduce runtime cost compared against the same sizes out of the Dv3-series.

Additionally it is valueable to get the input from the developers who is developing the application about what they believe is the requirement for a worker node GPU/CPU/Memory/IOPS wise. Based on this output you should select the given 't-shirt' side of the worker node. 

# Links

* [Cluster Autoscaler on Azure Kubernetes Service (AKS) - Preview](https://docs.microsoft.com/en-us/azure/aks/autoscaler)
* [Horizontal Pod Autoscaler Walkthrough](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)
* [Use Virtual Kubelet with Azure Kubernetes Service (AKS)](https://docs.microsoft.com/en-us/azure/aks/virtual-kubelet)
* [Sizes for Linux virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes)
