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
- [ ] :cloud: Capabilities of filtering network traffic with policies, azure firewall or network appliances
- [ ] :cloud: Container sandboxes, gVisor, kataContainers
- [ ] :fire: Maintaining secrets in HashiCorpVaul, Azure KeyVault, Azure KMS Plugin