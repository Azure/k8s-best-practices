# Securing a cluster

    > Understanding the cluster attack surface
    > Concepts that can be applied to configure and bootstrap authentication in azure
    > Minimizing the blast radius by applying least priviliges inside and outside the cluster
    > Securing and maintaining host vms
    > Monitoring and securing security events and logs

Table of Contents
=================

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

## Master Endpoint Security

The best thing to do is to ensure that RBAC with Azure AD Integration is configured. The API Server has three security levels. The first level is Authentication, the second level is Authorization, and the third level is Admission Controllers. By integrating Authentication and Authorization to Azure AD it means that only users from and Organization's Azure AD, and been granted the appopriate permissions, can access the API Server. If not, they are denied. The last layer around Admission Controllers is about applying policy to ensure that the permitted requests that do come through are adhering to the policy of the Organization. Admission Controllers are discussed more in the securing workloads section.

## Securing Host Access

By default, SSH access to the Control Planes is not permitted as AKS is a managed service. In terms of the workers, SSH access from the Public Internet is blocked by default via Network Security Groups (NSGs). Even if someone was able to get inside of the VNET, they would need to have the SSH Keys to be able to access the servers.

The recommended practice is to generate your own SSH Keys ahead of Cluster creation and secure those keys in something like Azure Key Vault (AZK). This was the keys are not floating around or stored on a server for someone to use.

## Default RBAC Setup

??? - Dennis has some stuff that he has done that could go here.

## Continuous Security (Should this be called Image Management)

One could argue that Container Image Management is one of the most critical parts of using Containers. It all starts with DevOps and the ability to automate the deployment of Applications/Services into AKS. If you trust where the images are coming from and put policy restrictions in place so that only trusted images are deployed into the AKS Cluster, that is half the battle.

If we can only deploy from a Trusted Registry then that eliminates the threat of deploying a malicious container from an untrusted registry. So how do we trust the registry? It all starts with a solid DevOps pipline and continuous security in the form of Container Image scanning at Build time as well as Runtime. If we scan Container Images at Build Time and only upload trusted images to the Registry, that means we are only pulling/deploying trusted images to our AKS Cluster. But what if a Container Image is comprimised at runtime, somehow someone has got access to a Container via some exploit? This is where Runtime Image scanning comes into play and let's you know that a Container might have a vulnerability. The policy could be simply to Notify someone, or it can be automated to recycle the Container which would immediately terminate the comprimised Container as the process would be killed and restarted.

## Dev Namespace Example

The following is an example of how to configure a Namespace, in this case we will call it **dev**. The goal is to apply LimitRanges, Quotas and RBACk to the Namespace to ensure it does not become a noisy neighbour and only consume the resources it has intended to along with providing security isolation so that it can only interact with the resources it is intended too.

```bash
kubectl apply -f securing_a_cluster/create-namespaces.yaml
kubectl apply -f securing_a_cluster/namespace-limitranges.yaml
kubectl apply -f securing-a_cluster/namespace-quotas.yaml
```

## Upgrading and Maintaing Hosts

??? - Not sure we can do anythig in this area today.

## Security Benchmarks

The purpose of running security benchmarks is to ensure that the proper policy, procedures and controls are in place when an AKS Cluster is created. By running a simple check we can ensure things are ready to go and in place from a security perspective.

Kubebench is a great example of one of the tools, click [here](https://github.com/aquasecurity/kube-bench) for more details.

## Certificate Maintenance and Rotation

??? - Rolling certificates in AKS today is not possible as access to the Control Plane is not allowed. It is on the roadmap, but not sure what, if anything, we do in the interim.

## Activating Add-ons

The recommended practice in this area is to enable monitoring so that logs are captured and can be used for troubleshooting down the road. Given there is no SSH access to the Control Plane and SSH access to the worker nodes is limited, having a solid Monitoring and Loggins strategy in place is key to Troubleshooting. A centralized loggin store is a good place to start.

As for the Kubernetes Dashboard, the recommendation is to not install it. If a solid DevOps pipline is in place along with a Loggin strategy, there is not need to provide access to the Kubernetes Dashboard. Misconfiguration of the Dashboard can be a big security risk, allowing users to have admin level access to do and see everything in the AKS Cluster.

## Encrypted Service to Service Communication

The question that usually gets raised, what about traffic inside of the cluster? The question back is what vulnerabilities/risks are we trying to mitigate? If we trust services that have got through the front door, do we need to do anything more? This is uncomfortable for a lot of Organizations as they are used to all service to service communication going through a WAF and monitoring traffic. When using Distributed Systems it does not make sense to have each request go back out to the WAF and come back in as the WAF is not aware of where Services are located, which is also one of the main reasons we use Ingress Controllers as they are Service Discovery aware.

So how do we deal with this trust problem? Typically the risk can be mitigated with the use of mutual TLS (mTLS) between services so that services can only interact with other trusted services. This is where the concept of Service Meshes come in. The details of Service Meshes are a topic for another time.

## Private API Server and Service Endpoints

Many Organizations are concerned that the Kubernetes API Server is Publicly exposed and are asking for a Private Endpoint. Private Endpoints are not an option today, but are on the roadmap. The ability to lock down access to the Public Endpoint to only certain VNETs (Service Endpoints) is also not available today, but on the roadmap.

**A bit of a duplicate of the first API Server section.**

So what can I do in the interim? The best thing to do is to ensure that RBAC with Azure AD Integration is configured. The API Server has three security levels. The first level is Authentication, the second level is Authorization, and the third level is Admission Controllers. By integrating Authentication and Authorization to Azure AD it means that only users from and Organization's Azure AD, and been granted the appopriate permissions, can access the API Server. If not, they are denied. The last layer around Admission Controllers is about applying policy to ensure that the permitted requests that do come through are adhering to the policy of the Organization. Admission Controllers are discussed more in the securing workloads section.