# Kubernetes 

My notes in installing and running Kubernetes on my local device and migrating over the docker services to a node. 

This assumes you already have the DPI repo cloned to your local and have the docker services successfully running. 

*If you do not have the DPI repo running, please see https://github.com/CSU-ITC303-ITC309-2023-Team8/broker* 

## Reference Books 

Available in the `#kubernetes` channel in the Discord server 

- Hands-On Docker for Microservices with Python 
- Hands-On Microservices with Kubernetes 

## Current Knowledge 

- Docker is a containerisation technology, allows an application and all its dependencies to run as a standalone executable package 
- Docker Compose is a tool for running multi-container Docker applications – this is currently how IoTa works. 
- Kubernetes is a system for deploying and managing containers at scale 
- By moving each of the IoTa services out of the Docker Compose file and into their own yaml file should enable Kubernetes to load each service separately 

## Installing Kubernetes 

To run kubernetes on my local device, I am using `minikube` and `kubectl` 

- Setup for `minikube` available at https://minikube.sigs.k8s.io/docs/start/  
- Setup for `kubectl` available at https://kubernetes.io/docs/tasks/tools/  

## Converting Docker Compose to Kubernetes 

I am using `kompose` for the conversion 

- Setup for `kompose` available at https://kompose.io/  

Running `kompose convert –f docker-compose.yaml` on the file at `/broker/compose` caused the error: 

- *FATA Unable to load files: service "frred" refers to undefined volume ${DATABOLT_SHARED_DIR}/raw_data: invalid compose project* 

Removing the `frred` entry from the `docker-compose.yaml` file and executing the `kompose` command then created a bunch of yaml files in the compose folder.

## Running the Microservices 

Start `minikube` with this command: 

- `minikube start` 

It may take a few minutes to get running, this will be the node that runs each of the microservices as a pod. 

In the folder with the microservice yaml files, run this command: 

- `kubectl apply -f .`

*Note: There is a period at the end of the above command. As the docker-compose.yml file still exists in the compose folder, there should be an error with Kubernetes try to execute this file as a single service.* 

To see which nodes are running: 

- `kubectl get nodes`

To see which pods are running: 

- `kubectl get pods` 

To show the Kubernetes Dashboard: 

- `minikube dashboard` 