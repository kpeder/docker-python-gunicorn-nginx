# Docker Image : python-gunicorn-nginx

[![license](https://img.shields.io/github/license/matthieugouel/docker-python-gunicorn-nginx.svg)](https://github.com/matthieugouel/docker-python-gunicorn-nginx/blob/master/LICENSE)


This image contains Nginx and Gunicorn on top of Python3 docker image.
These two software are managed with Supervisor.

## Usage

The entry point of your application must be named as **run.py**. Moreover, the instance in that file must be called **app**.
Also, by default the worker class used is Gevent.

You can include a custom Gunicorn configuration file into your application. By default the configuration file must be name as **gunicorn.config.py**

## Dockerfile example

Here is an example of a Dockerfile using that image :

```
FROM matthieugouel/python-gunicorn-nginx:latest
MAINTAINER Matthieu Gouel <matthieu.gouel@gmail.com>

# Copy the application
COPY . /app

# Install application requirements
RUN pip install -U pip
RUN pip install -r /app/requirements.txt
```

There is also a Flask demo application in the *app* folder of this repository.

## Build and run

First, build your image based on your Dockerfile.

```
docker build -t prod_api .
```

Then, you can run a container like this :

```
docker run -p 127.0.0.1:8000:80 prod_api
```

And finally access it with curl for instance :

```
curl localhost:8000
```
## Kubectl / Azure Kubernetes Service (AKS) deployment

The deployment is designed to use an nginx-ingress controller; for example, to run the Azure ingress deployment :
```
kubectl apply -f aks_ingress.yaml
```

The aks_deploy.yaml file creates a new deployment from image in Kubernetes like this :

```
kubectl apply -f aks_deploy.yaml
```
## Kubectl / Google Kubernetes Engine (GKE) deployment

Before performing the deployment you must grant your user the ability to create authorization roles :
```
kubectl create clusterrolebinding cluster-admin-binding \
--clusterrole cluster-admin --user $(gcloud config get-value account)
```
For reference, see [Kubernetes Engine > Role-Based Access Control > Prerequisites for using Role-Based Access Control](https://cloud.google.com/kubernetes-engine/docs/how-to/role-based-access-control#prerequisites_for_using_role-based_access_control).

Then run :

```
kubectl apply -f gke_ingress.yaml
```
and
```
kubectl apply -f gke_deply.yaml
```

## Kubectl / MiniKube deployment

Run minikube locally to develop and test container deployments in a Kubernetes cluster emulator.

Minikube installation reference : https://kubernetes.io/docs/tasks/tools/install-minikube/


