### Deploy an application using Consul on Minikube

### Prerequisites

* kubectl
* helm
* minikube

Now let's see How to do that:

1. Start Minikube

```
minikube start --memory 4096

minikube status
```

minikube had to download the few megabytes of dependencies and container images.

2. Start Minikube Dashboard

```
minikube dashboard
```

3. Install Consul with the official Helm chart

```
helm repo add hashicorp https://helm.releases.hashicorp.com

kubectl get namespace  
```

create one file named as helm-consul-values.yaml. Below is the code for that file :

```
cat > helm-consul-values.yaml <<EOF
global:
  name: consul
  datacenter: dc1
server:
  replicas: 1
  securityContext:
    runAsNonRoot: false
    runAsGroup: 0
    runAsUser: 0
    fsGroup: 0
ui:
  enabled: true
  service:
    type: 'NodePort'
connectInject:
  enabled: true
controller:
  enabled: true
EOF
```

```
helm repo add hashicorp https://helm.releases.hashicorp.com

helm install --values helm-consul-values.yaml consul hashicorp/consul --version "0.40.0"
```

4. List services

```
minikube service list

minikube service consul-ui
```

5. Deploy services with Kubernetes

deploy a two-tier application made of a backend data service that returns a number (the counting service), and a frontend dashboard that pulls from the counting service over HTTP and displays the number.

first, we’ve deployment, service, and service account for the counting service.

```
kubectl apply -f counting.yaml
```

Create a deployment definition, service, and service account for the dashboard service named dashboard.yaml

```
kubectl apply -f dashboard.yaml
```
**NOTE**  "deploy both on minikube"

6. Port Forward

```
kubectl port-forward deploy/dashboard 9002:9002
```