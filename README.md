# minikube-example

Example of Minikube deployment with two services (backend and frontend). An example image is used and no interaction between frontend and backend is made beacuse of it.

## Install minikube and kubectl

```bash
pacman -S minikube kubectl
```

## Launch minikube

```bash
minikube start
```

## Deploy Kubernetes UI

### Deploy Kubernetes UI

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

### Create a sample admin-user

```bash
kubectl apply -f create-user.yml
```

### Get token to access UI

```bash
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```

### Log in with the token in

```bash
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

## Previous Config

- Namespace
- ConfigMap with coreDNS configuration changes (Backend DNS)

```bash
kubectl apply -f previous-config.yml
```

## Backend

- Service
- Deployment (3 pods)
- Custom dns name through coreDNS plugin configuration
- ConfigMap passed to all pods

```bash
kubectl apply -f backend.yml
```

## Frontend

- Service
- Deployment (3 pods)
- Backend DNS name as environment variable
- Ingress created ponting to Service with nginx ingress plugin

Note: I ran it locally. Modified /etc/hosts for resolving external dns name as minikube IP.

```bash
kubectl apply -f frontend.yml
```
