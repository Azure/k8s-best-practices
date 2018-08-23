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