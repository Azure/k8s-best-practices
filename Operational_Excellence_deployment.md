# Deployment
Objectives:

    > Using Infrastructure as Code for declarative and reusable deployments
    > Using helm charts for complex application deployments
    > Find the best VM size to fit the workload requirements

Table of Contents
=================

* [Deployment](./Operational_Excellence_deployment.md)
    * Infrastructure as Code
        * Azure Resource Manager templates
        * Terraform
    * Helm charts
    * Lighter Helm alternative - kontemplate
    * [Choose the right VM size for your nodes](./Cost_Optimization.md#node---vm-sizes)

# Infrastructure as Code

## Azure Resource Manager templates

## Terraform

Terraform provides first-class support to define your resources in Azure in a
declarative way. The configuration language is concise and very well adapted to
pull-request-based work models where any infrastructure change is first
reviewed before being deployed.

See here for the documentation on Azure resources that can be managed by Terraform:
<https://www.terraform.io/docs/providers/azurerm/>

We recommend setting up only the Azure resources with Terraform, and not what
runs "inside" them. Think of it really as preparing the infrastructure for your
workloads. For example, after you have created your VMs with Terraform, you
should configure operating-system settings with a tool such as Ansible. Also
for Kubernetes/AKS, we recommend using something like Helm charts / Kustomize,
and not Terraform.

You should store the Terraform state in Azure storage, so that the state is
near to the managed resources. This is particularly important if multiple
persons run Terraform, so that you have proper locking and accounting of the
resources.

# Helm charts

# Lighter Helm alternative - kontemplate

[Kontemplate](https://github.com/tazjin/kontemplate) is lighter and easier than Helm, which makes it a good alternative for projects that do not require a more complex solution. It is a simple CLI tool that can take sets of Kubernetes resource files with placeholders and insert values per environment. It works with standard Kubernetes YAML and/or JSON manifest files with templating instructions in a format based on Go's templating engine. It also has some interesting features like integration with kubectl (e.g. the --dry-run flag) and [pass](https://www.passwordstore.org), which makes it work well with CI pipelines.
