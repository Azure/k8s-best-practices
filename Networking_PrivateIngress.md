# Private ingress

    > Running services with an ingress behind an internal load balancer

Table of Contents
=================

* [Networking](./Networking.md)
  * Private Ingress

You may want to use an Ingress controller to expose services, because you want to use its HTTP Layer 7 load balancinc features, SSL terminations, etc. but do so over an internal IP without exposing it externally.

An Ingress controller creates a Kubernetes Service in the background, so the trick here would be to create a new Ingress controller and annotate it to create a Service backed up by an Internal Azure Load Balancer.

For this example, you're going to provision a new nginx ingress controller using Helm. You can take a look at the [stable/nginx-ingress](https://github.com/helm/charts/tree/master/stable/nginx-ingress) repository to figure out all the parameters but essentially, there are two important ones:

1. `controller.ingressClass`: you'd want to set this to a value that you'll use later on, like `nginx-internal`
1. `controller.service.annotations`: you'll use this to annotate the backing Kubernetes Service to use an internal load balancer.

Create a [`values.yaml`](networking/internal_ingress_values.yaml) file with the content below:

```yaml
controller:
  ingressClass: nginx-internal
  service:
    annotations:
      service.beta.kubernetes.io/azure-load-balancer-internal: "true"
```

For RBAC-enabled AKS clusters run the command below:

```sh
helm install stable/nginx-ingress --name nginx-ingress-internal --namespace ingress-controllers -f values.yaml
```

For AKS clusters with RBAC disabled, run the command below:

```sh
helm install stable/nginx-ingress --name nginx-ingress-internal --namespace ingress-controllers -f values.yaml --set rbac.create=false
```

Wait for the new internal Azure Load Balancer to be provisioned and note the "external" IP, which is actually a private IP from your Virtual Network.

```sh
kubectl get services nginx-ingress-internal-controller  --namespace ingress-controllers -w

NAME                                TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
nginx-ingress-internal-controller   LoadBalancer   10.0.167.34   10.240.0.66    80:30291/TCP,443:31842/TCP   31s
```

Deploy [the sample](networking/sample_internal_ingress_deployment.yaml) below. Notice the `ingress.class` used.

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: frontend
  name: frontend
spec:
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: nginx:1.7.9
        imagePullPolicy: Always
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: frontend
  name: frontend-service
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: frontend
  type: ClusterIP
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx-internal
  name: frontend-ingress
spec:
  rules:
    - host: www.example.com
      http:
        paths:
          - backend:
              serviceName: frontend-service
              servicePort: 80
            path: /
```

Finally test by running a pod and using it to curl the internal ingress IP:

```sh
kubectl run -it --rm aks-ingress-test --image=debian
```

Install `curl` in the pod using `apt-get`:

```sh
apt-get update && apt-get install -y curl
```

Now use `curl` to access the internal IP of your ingress controller, providing the Host Header information as per the ingress specification. Note that your IP address may be different than the below:

```
curl -L http://10.240.0.66 -H "Host: www.example.com"
```

You should get the nginx welcome page

```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
```