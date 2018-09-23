# Deployment

    > Keeping the AKS cluster up-to-date with security patches
    > Upgrading the AKS cluster to a new Kubernetes version

Table of Contents
=================

* [Maintenance](./Operational_Excellence_maintenance.md)
    * Patch management
    * Upgrade management

# Patch management

The nodes are configured to install and apply security patches on a nightly schedule. Some patches require a node reboot to be applied. The best practice here is using the open-source reboot daemon called kured.

-> https://github.com/weaveworks/kured

For RBAC-enabled AKS clusters you must apply the [kured-rbac.yaml](https://github.com/weaveworks/kured/blob/master/kured-rbac.yaml) first. Otherwise the DaemonSet will not run correctly. Afterwards deploy the kured daemonset via the [kured-ds.yaml](https://github.com/weaveworks/kured/blob/master/kured-ds.yaml) in your cluster. But take a look first at the container registry https://quay.io/repository/weaveworks/kured?tab=tags to get the latest tag that needs to be adjusted in the deployment template.

```yaml
...
    spec:
      containers:
        - name: kured
          image: quay.io/weaveworks/kured:master-5731b98
          imagePullPolicy: IfNotPresent
...
```

Using the latest image is very important regarding the kubectl version that the kured image contains. As you may know the kubectl version should always match the Kubernetes version of the AKS cluster.

Per default settings the kured DaemonSet runs in the kube-system namespace.

Kured checks every hour for the file /var/run/reboot-required and then initiates the reboot of the node. The daemon uses the cordon and drain process to safely reboot the node.

-> https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

Ensuring that only one node at a time enters the reboot process, kured works with locks.

```
time="2018-04-24T13:56:29Z" level=info msg="Kubernetes Reboot Daemon: master-5731b98"
time="2018-04-24T13:56:29Z" level=info msg="Node ID: aks-agentpool-14987876-1"
time="2018-04-24T13:56:29Z" level=info msg="Lock Annotation: default/kured:weave.works/kured-node-lock"
time="2018-04-24T13:56:29Z" level=info msg="Reboot Sentinel: /var/run/reboot-required every 1h0m0s"
time="2018-04-24T13:56:31Z" level=info msg="Holding lock"
time="2018-04-24T13:56:31Z" level=info msg="Uncordoning node aks-agentpool-14987876-1"
time="2018-04-24T13:56:36Z" level=info msg="node \"aks-agentpool-14987876-1\" uncordoned" cmd=/usr/bin/kubectl std=out
time="2018-04-24T13:56:36Z" level=info msg="Releasing lock"
```
```
time="2018-04-24T12:29:42Z" level=info msg="Kubernetes Reboot Daemon: master-5731b98"
time="2018-04-24T12:29:42Z" level=info msg="Node ID: aks-agentpool-14987876-2"
time="2018-04-24T12:29:42Z" level=info msg="Lock Annotation: default/kured:weave.works/kured-node-lock"
time="2018-04-24T12:29:42Z" level=info msg="Reboot Sentinel: /var/run/reboot-required every 1h0m0s"
time="2018-04-24T13:55:33Z" level=info msg="Reboot required"
time="2018-04-24T13:55:33Z" level=warning msg="Lock already held: aks-agentpool-14987876-1"
```

# Upgrade management

