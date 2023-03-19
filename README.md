# k8-demo

Playground for kubernetes testing

# Instalation Setup for Windows

## Download Docker desktop and enable kubernetes

```
https://docs.docker.com/desktop/install/windows-install/
```

## chocolatey package manager (optional) (Powershell)

```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

## Install kubernetes cli minikube

```
choco install kubernetes-cli
choco install minikube
```

## Common Kubernetes Commands

Create Namespace

```
kubectl create namespace mynamespace
```

To create nodes/multiple nodes

```
minikube start
minikube start --nodes 2 -p nodes
```

View list of nodes

```
kubectl get nodes
```

View node status

```
minikube status -p nodes
```

Creating containers (nginx image example)

```
kubectl create deployment my-nginx --image nginx
```

View pods

```
kubectl get pods
kubectl get pods -o wide
kubectl get pods --namespace mynamespace -o wide
```

View Deployments

```
kubectl get deployment
kubectl describe deployment my-nginx
```

Scale Deployments

```
kubectl scale deployment my-nginx --replicas 4
```

Expose Ports

```
kubectl expose deployment my-nginx --port=80 --type=NodePort
```

Port Forwarding

```
kubectl port-forward podname 80:80
```
