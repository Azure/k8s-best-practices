# Securing workloads
Objectives:
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

For multitentant clusters it is useful to enforce the image pull policy to Always - see AlwaysPullImages admission controller documentation https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#alwayspullimages 

## Admission controlers

An admission controller is a piece of code that intercepts requests to the Kubernetes API server prior to persistence of the object, but after the request is authenticated and authorized. The controllers consist of the list below, are compiled into the kube-apiserver binary, and may only be configured by the cluster administrator. 

AKS supports the following admission controllers: https://docs.microsoft.com/en-us/azure/aks/faq#what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed

### DenyEscalatingExec 

> Usefullness?

### Pod security policies

A `Pod Security Policy` is a cluster-level resource that can enforce rules on security aspects of a pod specification and affect the `SecurityContext`that will be applied to a pod and and container. It will be stored as an configuration object that defines conditions that a pod must run with in order to be accepted into the cluster.

> AKS does not support Pod Security Policies today.

## Maintaining secrets

Generally speaking there are two options to store and secure secrets/ connection strings in AKS.
1. Using the default mechanism for configuring and mainting secrets via the secret object model :https://kubernetes.io/docs/concepts/configuration/secret/  
2. Keeping secret information not in your cluster but in azure keyvault

While the first option of using kubernetes native secret management has the obvious advantage of simplified operations it incurs the risk of credential leakage if your cluster is breached or someone with existing cluster access uses his privileges to access to retrieve connections strings and secrets. Therefore this approach requires the lockdown of access to secrets via RBAC and a good way of recycling and upgrading secret information on a regular basis. In addition the encryption of etcd can be performed by using azure keyvault to secure the encryption key - this protects the retrieval of secret information from attackers that can access the environment.
Kubernetes KMS plugin: https://github.com/Azure/kubernetes-kms 

The second approach can further improve the security setup by ensuring that all secret information is stored within the azure keyvault. Each pod therefore has to authenticate to azure ad and retrieve its assigned secrets information upon startup and refresh the access regularly. This option can also be used to ensure that all secret information is only available in memory and never written to disk - neither in etcd or environment variables. This can be combined with the usage of aad-pod-identity which makes use of azure managed service identity to grant a unique identity to each pod.
AAD Pod Identity: https://github.com/Azure/aad-pod-identity
KeyVault Flex Volume: https://github.com/Azure/kubernetes-keyvault-flexvol
Key Vault Agent: https://github.com/Hexadite/acs-keyvault-agent 

## Network policies

A network policy is a specification of how groups of pods are allowed to communicate with each other and other network endpoints.
This allows for locking down traffic from/to specific sets of pods according to metadata/namespace assignment based on IP rules. For the enforcement of dns based rules the usage of a service mesh technology like Istio can be usefull.

As of today AKS supports the following options for enforcing network policies:
- https://github.com/cloudnativelabs/kube-router
- https://github.com/Azure/azure-container-networking/tree/master/npm

Good sample network policies recipes: https://github.com/ahmetb/kubernetes-network-policy-recipes 
