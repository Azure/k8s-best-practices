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
        * Lighter Helm alternative - kontemplate
    * Helm charts
    * [Choose the right VM size for your nodes](./Cost_Optimization.md#node---vm-sizes)

# Infrastructure as Code

## Azure Resource Manager templates

## Terraform

## Lighter Helm alternative - kontemplate

[Kontemplate](https://github.com/tazjin/kontemplate) is lighter and easier than Helm, which makes it a good alternative for projects that do not require a more complex solution. It is a simple CLI tool that can take sets of Kubernetes resource files with placeholders and insert values per environment. It works with standard Kubernetes YAML and/or JSON manifest files with templating instructions in a format based on Go's templating engine. It also has some interesting features like integration with kubectl (e.g. the --dry-run flag) and [pass](https://www.passwordstore.org), which makes it work well with CI pipelines.

# Helm charts