# Setting up environments
Objectives:
    > Concepts that can be applied to ensure security isolation for different workloads
    > Separating Subscriptions, Resource Groups, Azure RBAC, Service Accounts and Secret

Table of Contents
=================

* [Setting up environments](./Security_setting_up_environments.md)
    * Separating environments
    * Setting up/ Validating virtual network
    * Provisioning clusters 

- [ ] :fire: Cluster vs Nodes vs Namespace isolation
- [ ] :fire: Azure service principals and MSI
- [ ] :fire: Inbound/ Outbound traffic control
- [ ] :fire: Setting up RBAC roles and default bindings

## Cluster vs Nodes vs Namespace Isolation

Some thought needs to get put into how you are going to isolate environments and workloads. For example, an organization can use a single cluster and isolate different environments and workloads using Namespaces. Another example to separate environments is to have a cluster per environment, or one cluster for Production and one for non-Production. If Network isolation is of the utmost importance then having a cluster per network zone is another option.

There is not right or wrong answer as it depends on the needs of the organization. The biggest thing is to balance operations and maintenance with practicality. Yes having a separate cluster for each network zone satisfies a requirement, but on the flip side there are more clusters to manage and maintain which means incurring more technical debt.

A general rule of thumb is to have a Production Cluster and a non-Production Cluster at a minimum. The workloads (microservices) within each cluster are separated via K8s Namespaces in order to apply LimitRanges, Quotas and RBAC so that one service does not be a noisy neighbour to another service and ensure the creation of governance boundaries between different workloads. Keep also in mind that a testing infrastructure for evaluating the impact of Kubernetes version upgrades needs to be available in order to validate changes in the api version and behaviour of Kubernetes.

## Azure Service Principals and MSI

Today, controlling what an AKS Cluster can do within an Azure Subscripiton is done via Service Principals. Meaning, the cluster can only provision or interact with resources it is allowed to access. The recommended practice is to create the Service Principal ahead of time and only grant it the minimal permissions it needs for creation of the AKS Cluster.

Let's use an example of how this works today. As we know, one of the recommended practices with Containers is to not store sensitive information within a Container Image, but leverage a store like Azure Key Vault (AKV) to store that sensitive information. But how does the service get access to AKV in order to retrieve said information? Today, with Service Principals that means storing the credentials to access AKV somewhere, which means they need to be managed. What if there was another way?

This is where Managed Service Identities (MSIs) come in. MSIs are a managed identity in Azure AD that allow an application/service to get access to other resources in Azure, AKV for example, without having to provide credentials. The credentials are managed on the service's behalf and allow the service identity to authenticate to any service that supports Azure AD authentication. It just so happens that Azure Key Vault is one of those service, meaning you don't hvae to store credentials in your code.

## Inbound/Outbound Traffic Control

Let's put the Kubernetes API Server aside for a second and focus on the Application Workloads. Getting access to Application Workloads from the Internet or Intranet is done via Ingress Controllers. The best way to think of Ingress Controllers is a Reverse Proxy that can help control traffic coming into the AKS Cluster. We could expose each service using an Internal Load Balancer or via a Public IP Address, but that would become hard to manage, especially if we want to have a single DNS Endpoint. Ingress Controllers help us to focus all interal or external traffic through a few key exposed endpoints.

Keeping the above in mind, those Public or Internal endpoints can be protected in the same way that endpoints are protected today, by putting a Web Application Firewll (WAF) in front of that endpoint and only allowing traffic to the Ingress Controller from the WAF. The WAF can then take of the OWASP vulnerabilities we have become accustomed to them taking care of for us. Example of how this can be setup can be found here: https://github.com/Azure/application-gateway-kubernetes-ingress. Another option can be to configure the Azure firewall to filter ingoing and outgoing traffic to dedicated urls.

The question that usually gets raised, what about traffic inside of the cluster? The question back is what vulnerabilities/risks are we trying to mitigate? If we trust services that have got through the front door, do we need to do anything more? This is uncomfortable for a lot of Organizations as they are used to all service to service communication going through a WAF and monitoring traffic. When using Distributed Systems it does not make sense to have each request go back out to the WAF and come back in as the WAF is not aware of where Services are located, which is also one of the main reasons we use Ingress Controllers as they are Service Discovery aware.

So how do we deal with this trust problem? Typically the risk can be mitigated with the use of mutual TLS (mTLS) between services so that services can only interact with other trusted services. This is where the concept of Service Meshes come in. The details of Service Meshes are a topic for another time.

## RBAC Setup and Default Bindings

One could argue that how RBAC is setup in an AKS cluster is the single most security control you can apply. RBAC controls what can be done via the API Server as well as inside of the AKS Cluster. The recommended way to implement RBAC within an AKS Cluster is by connecting it to Azure Active Directory (Azure AD). This allows an Organization to secure an AKS Cluster in the same manner as they secure access to an Azure Subscription.

At a minimum, Organizations should grant a Group in Azure AD Cluster Admin priviliges and another group in Azure AD read-only privileges. This ensure that the right folks in the Organization have admin access to the cluster and another group has read privileges to see what is going on. Outside of that, Organizations should look to setup specific RBAC per namespace that is setup in Kubernetes (K8s). Namespaces provide a great isolation boundary ensuring that one service does not impact another service or be a noisy neighbour.