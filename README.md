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

# Basic Jenkins Setup

Create Namespace

```
kubectl create namespace jenkins
```

Create basic jenkins-deployment.yaml file

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      containers:
      - name: jenkins
        image: jenkins/jenkins:lts-jdk11
        ports:
        - containerPort: 8080
        volumeMounts:
        - name: jenkins-home
          mountPath: /var/jenkins_home
      volumes:
      - name: jenkins-home
        emptyDir: { }
```

create the jenkins deployment

```
kubectl create -f jenkins-deployment.yaml -n jenkins
```

Create basic service.yaml file

```
apiVersion: v1
kind: Service
metadata:
  name: jenkins
spec:
  type: NodePort
  ports:
  - port: 8080
    targetPort: 8080
  selector:
    app: jenkins
```

Create the service

```
kubectl create -f service.yaml -n jenkins
```

Validate service creation

```
kubectl get services -n jenkins
```

Find name and ip of pod

```
kubectl get pods -n jenkins -o wide
```

Port forward to 8080

```
kubectl -n jenkins port-forward <pod_name> 8080:8080
localhost:8080
```

Get admin password for initial setup

```
kubectl logs <pod_name> -n jenkins
```
