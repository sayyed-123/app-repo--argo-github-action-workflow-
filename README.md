# MLOps using argo-workflow
This repo explain how to use ARGO to automate ML pipeline called MLOps.
# What is Argo Workflows?
Argo Workflows is an open source container-native workflow engine for orchestrating parallel jobs on Kubernetes. Argo Workflows is implemented as a Kubernetes CRD.
- Define workflows where each step in the workflow is a container.
- Model multi-step workflows as a sequence of tasks or capture the dependencies between tasks using a graph (DAG).
- Easily run compute intensive jobs for machine learning or data processing in a fraction of the time using Argo Workflows on Kubernetes.
- Run CI/CD pipelines natively on Kubernetes without configuring complex software development products.
# Why Argo Workflows?
- Designed from the ground up for containers without the overhead and limitations of legacy VM and server-based environments.
- Cloud agnostic and can run on any Kubernetes cluster.
- Easily orchestrate highly parallel jobs on Kubernetes.
- Argo Workflows puts a cloud-scale supercomputer at your fingertips!
# Quick Start
https://argo-workflows.readthedocs.io/en/latest/quick-start/
# Set up kubernetes cluster 
You could consider the following local Kubernetes cluster options:
- minikube
- kind
- k3s or k3d
- Docker Desktop# Download the binary
# Minikube set up on docker
## Install Docker Engine on Ubuntu
https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
## Install minikube
- https://minikube.sigs.k8s.io/docs/start/

$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

$ sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

### add your user to docker group to avoid permission error

  $ sudo usermod -aG docker $USER && newgrp docker
  
  $ minikube start
  
  $ minikube status
  
  $ alias kubectl="minikube kubectl --"
  
  $ kubectl get pods

# Install Argo Workflows  
https://argoproj.github.io/workflows/

https://argo-workflows.readthedocs.io/en/latest/quick-start/
## For Linux
### Download the binary
$ curl -sLO https://github.com/argoproj/argo-workflows/releases/download/v3.4.14/argo-linux-amd64.gz
### Unzip
$ gunzip argo-linux-amd64.gz

### Make binary executable
$ chmod +x argo-linux-amd64

### Move binary to path
$ sudo mv ./argo-linux-amd64 /usr/local/bin/argo

### Test installation
$ argo version
# Install Controller & Server for argo-workflow
$ kubectl create namespace argo

$ kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v3.4.14/quick-start-minimal.yaml

$ kubectl get all -n argo
# Submit an example workflow via CLI
$ argo submit -n argo https://raw.githubusercontent.com/argoproj/argo-workflows/main/examples/hello-world.yaml --watch
### You can list all the Workflows you have submitted by running 
$ argo list -n argo

# Submit argo-workflow via the UI
- Forward the Server's port to access the UI:

$ kubectl -n argo port-forward service/argo-server 2746:2746

$ ssh -i key.pem -L 2746:localhost:2746 -N ubuntu@<Public_ip_EC2>
- Navigate your browser to https://localhost:2746
![image](https://github.com/sayyed-123/argo-workflow/assets/166358159/544da679-e827-4b8e-af65-d4ee6ff106ae)
![image](https://github.com/sayyed-123/argo-workflow/assets/166358159/db763107-a26f-413d-80d6-bbdc44963d4b)

# build the docker image for each component of ML workflow
##  $ cd containers/etl/
$ chmod +x build.sh

$ ./build.sh <DOCKERHUB_USER_NAME>

##  $ cd containers/model_training/
$ chmod +x build.sh

$ ./build.sh <DOCKERHUB_USER_NAME>

##  $ cd conatiners/model_serving/
$ chmod +x build.sh

$ ./build.sh <DOCKERHUB_USER_NAME>

# Create bucket at any cloud and get credintial
### put scores.csv file in bucket ( for this project create google cloud bucket & key.json )

# Run Argo-workflow from command line

### $ argo submit -n argo pipeline.yaml --watch

![image](https://github.com/sayyed-123/argo-workflow/assets/166358159/0a71a827-1d4e-4575-98ac-ccde75fbe14f)

![image](https://github.com/sayyed-123/argo-workflow/assets/166358159/afb29f44-ef86-4b53-8487-6c0d0318811e)

## Now flask app is running in a Pod




