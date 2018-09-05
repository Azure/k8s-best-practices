# Securing workloads

    > Understanding the attack surface from container images and laying out Microsoft 1st party and ecosystem options for securing images
    > Ensuring minimal attack surface and good minimal security default on apps
    > Protecting secret information and workloads from each other
    > Isolating ingoing and outgoing traffic and monitoring relevant behaviour

Table of Contents
=================

*  [Securing workloads](./Security_securing_workloads.md)
    * DenyEscalatingExec, Pod identities, security contexts and pod security policies
    * Securing serviceAccounts and secrets
    * Network segmentation (Ingress/ Egress)
    * Secure images and admission controller
    * Container sandboxes
    * Managing secrets and privileged information

- [ ] :fire: Image scanning in azure container registry, ValidatingAdmissionWebhook and third party products like Twistlock, Neuvektor and Aqua
- [ ] :cloud: Ensuring DenyEscalatingExec adimission controllers/ pod security policies, privileged pods, runasroot, volumes, fsGroups, hostports on AKS / ACS-Engine
- [ ] :cloud: Maintaining secrets in HashiCorpVaul, Azure KeyVault, Azure KMS Plugin
- [ ] :cloud: Capabilities of filtering network traffic with policies, azure firewall or network appliances
- [ ] :cloud: Container sandboxes, gVisor, kataContainers

## Image protection

For multitentant clusters it is useful to enforce the image pull policy to Always - see AlwaysPullImages admission controller

## Admission controlers

An admission controller is a piece of code that intercepts requests to the Kubernetes API server prior to persistence of the object, but after the request is authenticated and authorized. The controllers consist of the list below, are compiled into the kube-apiserver binary, and may only be configured by the cluster administrator. 

AKS supports the following admission controllers: https://docs.microsoft.com/en-us/azure/aks/faq#what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed

### DenyEscalatingExec 

> Usefullness?


### Pod security policies

A `Pod Security Policy` is a cluster-level resource that can enforce rules on security aspects of a pod specification and affect the `SecurityContext`that will be applied to a pod and and container. It will be stored as an configuration object that defines conditions that a pod must run with in order to be accepted into the cluster.

> AKS does not support Pod Security Policies today.

## Maintaining secrets


KeyVault Flex Volume: https://github.com/Azure/kubernetes-keyvault-flexvol
Kubernetes KMS plugin: https://github.com/Azure/kubernetes-kms 
Key Vault Agent: https://github.com/Hexadite/acs-keyvault-agent 

## Network policies

A network policy is a specification of how groups of pods are allowed to communicate with each other and other network endpoints.
This allows for locking down traffic from/to specific sets of pods according to metadata/namespace assignment based on IP rules. For the enforcement of dns based rules the usage of a service mesh technology like Istio can be usefull.

As of today AKS supports the following options for enforcing network policies:
- https://github.com/cloudnativelabs/kube-router
- https://github.com/Azure/azure-container-networking/tree/master/npm

Good sample network policies recipes: https://github.com/ahmetb/kubernetes-network-policy-recipes 
