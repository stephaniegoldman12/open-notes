Deploying one docker container, then two, then need to add load balancer
Then you get a ton of traffic, need to have 100 docker containers and 3 load balancers
This is where K8s (a container orchestration tool) comes in

## K8s
Kubernetes helps you make Docker better
Master: tells your servers what to do

The master server has:
- K8s API server
- Scheduler
- Controller manager
- etc.

Added to each of the three servers (worker node):
Will have one container per pod
- Kube-Proxy
- Kubelet
- Docker
- OS (Ubuntu)
- Hardware

Can create a K8s cluster in Linode and get the master API endpoint

- Get KubeCTL command line
- Kubectl run command
- Kubectl describe pods


Deployment
- 3 pods: each with an IP address (because you can deploy pods like crazy, master can monitor so if at 75% utilization can deploy another)
- nccoffee
- port 80
  => specify all of this in a yaml file (often called manifests - after "ship manifest")

Service: acts as a load balancer between the pods
- Put in a service.yaml
- "Node balancer" -> can deploy at 10 IP addresses
  
Edit deployment:
- Kubectl edit deployment 
- 1. Update docker image
- 2. Update manifest file to deploy across 20 IP addresses
  
What's a helm chart?
- Package manager for Kubernetes -> package collections of Kubernetes yaml files
- Create a bundle of YAML files and push them to a helm repo so others can use it 
- `helm install <chart_name>` to reuse a configuration
- helm hub -> public registration
- It's a templating engine. Instead of writing yaml file for each microservice. Using helm you can define a blueprint 

name: {{ .Values.name }}
name: {{ .Values.container.name }}
image: {{ .Values.container.image }}
port: {{ .Values.container.port }}