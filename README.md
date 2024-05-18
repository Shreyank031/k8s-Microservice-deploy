# Voting App

This is a distributed voting application built using various technologies and deployed on Kubernetes. The app allows users to vote for their favorite choice (Cat or Dog) and displays the live results.

## Architecture

The application follows a microservices architecture and consists of the following components:

1. **Frontend**: A Python-based web application that serves as the user interface for voting.
2. **Redis**: An in-memory data store used for storing and retrieving votes.
3. **PostgreSQL**: A persistent database for storing and retrieving voting results.
4. **Worker**: A .NET service that consumes votes from Redis and persists them to PostgreSQL.
5. **Result**: A Node.js application that fetches and displays the voting results from PostgreSQL.

## Microservices

The application is designed as a collection of microservices, each responsible for a specific task. This architecture promotes scalability, resilience, and flexibility, as each microservice can be developed, deployed, and scaled independently.

Each microservice is deployed as a separate Kubernetes Deployment, which ensures that the desired number of replicas is running at all times. Kubernetes Deployments provide features like rolling updates, rollbacks, and self-healing capabilities.

## Deployment

The microservices are deployed on a Kubernetes cluster using Deployment and Service manifests:

1. **Deployment Manifests**: These YAML files define the desired state of each microservice, including the container image, resource requirements, and other configurations.

2. **Service Manifests**: Kubernetes Services expose the microservices within the cluster and handle service discovery, load balancing, and networking.

The Deployment and Service manifests for each microservice are located in the `k8s-Microservice-Deploy/deployments/` and `k8s-Microservice-Deploy/services/` directories, respectively.


>> I have also included the pods for all five componenets, in case you want to test it, understand before going to deployemnt.


## Docker Images

The Docker images for each microservice are pulled from Docker Hub  during the deployment process. The image names and tags are specified in the respective Deployment manifests.

For example, the Frontend Deployment manifest (`k8s/deployments/frontend-deployment.yaml`) might include the following image specification:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: 
  name: voting-app-deployment
  labels:
    name: voting-app-deployment
    app: voting-app
spec:
  template:
    metadata:
      name: voting-app-pod
      labels: 
        name: voting-app-pod
        app: voting-app
    spec:
      containers:
      - name: voting-app
        image: kodekloud/examplevotingapp_vote:v1 #Image will pulled form dockerhub
        ports:
        - containerPort: 80
  replicas: 1
  selector:
    matchLabels:
      name: voting-app-pod 
      app: voting-app
```

## Prerequisites

- Docker
- Minikube (or any other Kubernetes cluster)
- kubectl (Kubernetes command-line tool)
