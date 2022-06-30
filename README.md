# KUBERNETES

Kubernetes is an open source Container Orchestration Tool developed by google. Basically it manages containers, it could be docker containers or any other techonology. It helps to manage applications that are made up of 100s or 1000s of containers and helps to manage in different environments.

Features it offers.

- High avaliability
- Scalability
- Disaster recovery

## KUBERNETES COMPONENTS

https://www.youtube.com/watch?v=X48VuDVv0do&t=320s

### POD

- Smallest unit of k8s. It is a type of abstraction over the container that actually runs the service. Using this you don't have to communicate with the container and just manage the k8s layers. Usually 1 application per pod.
- All pods are assigned its own ip address in the internal virtual network and each pod can communicate using these ip address with another pod.
- Pods are ehimeral i.e they can easily but to make sure everything runs smoothly a new pod is created automatically but it will be assigned a new ip address and this could create problem as pods will be communicating with those ip address.
- To resolve this problem service is used.

### SERVICE

- It is basically a static ip address that can be attached to each pod. Even if the pod dies service will still be running and you won't have to be worried about new ip address for the new pod that is taken by the service.

#### External service

serivce available to outside network

#### Internal service

Service only available to other internal services

### INGRESS

- Now a service is acutally assigned an ip address and to access it from outside you have to use but that is not practical. So to make it possible for it to be used using simple names we make use of another component which is Ingress.
- Any request made by outside application to the external service first goes to the Ingress and then it is routed to the particular service.

### CONFIGMAP

- Suppose if you have an application service and db service. Now to communicate from our application to the database we would require some configurations like serivce url and other stuff.
- Now these configs can change while serivce build or while deployment and if you store all the configurations directly in the application that you would have to change it each time this happens and rebuild the container image and container.
- To make sure this does not happen we have ConfigMap which we can use to store all the external configurations required by application.

### SECRET

- Now username and passwords for the db service will also come under external service but you can't store them directly in the configMap as plain text as it will be insecure.
- And here comes the Secret, it can be used to store external credentials or any other secret data ( keys, passwords, certificates etc ).
- The data is stored in base64 format.

## DATA STORAGE IN KUBERNETES

If a db pod restarts then you will all the data stored on that pod and yes, that is not the expected behaviour. To resolve this issue we use volumes.

### VOLUMES

- It can be used to store data persistently or permanently.
- It basically attaches a physical or virtual storage to the pod. It can be available in the same network or a remote storage.
- The pod just have an external reference to the storage.
- It is not part of the K8s cluster and a cluster doesnot manage any persistent data storage on its own.

### DEPLOYMENT

- Now suppose after deployment the application crashes and pod or services die. Now if someone is trying to access the application. They won't be able to and it will bad user ecperience.
- To resolve this issue we could keep mulitple instances of the application running nad if anyone of these die than the traffic can be redirected to other instances. And services defined above does all this work and runs multiple replicated instances of the application or db pods are running.
- Now to create multiple instances of a pod you won't actually be creating multiple pods seperately instead you will define a blueprint that can be used to create a pod and then define the number of instances you want to start.
- That component or blueprint is known as Deployment. And as a administrator you actually work with deployments.
- You won't be communicating directly with any of the components and will just be creating deployments.

### STATEFUL SET

- Now the above same scenario can be observed with db pods and we have to make sure we have multiple instances of db running.
- Now a normal deployment cannot be used for creating db instances because db instances need to be connected with external volumes to make data persistent.
- And we want some kind of way through which we can keep track of which services are communicating with the database and which services are reading and writing data to the database to avoid inconsistencies in the data.
- The mechanism we use for this purpose along with replication feature is known as Stateful Set.
- So this component is mainly for databases. i.e mqsql, mongodb etc
- It is slightly difficult to create database application using statefull sets.

## KUBERNETED ARCHITECTURE

https://www.youtube.com/watch?v=X48VuDVv0do&t=1349s

## MINICUBE AND KUBECTL

https://www.youtube.com/watch?v=X48VuDVv0do&t=2087s

### MINICUBE

1 Node kubernetes cluster that runs on your local machine and can be used for testing kubernetes on local setup.

### KUBECTL

Now with minicube you have access to a node on local setup and you want interact with the cluster and kubectl is used for that. It is a commandline tool for kubernetes cluster.

## KUBECTL COMMANDS

`kubectl get pod` returns a list of pods running
`kubectl get services` return a list of services running  
`kubectl create deployment NAME --image=image [--dry-run] [options]` creates a new deployment with provided image
`kubectl create deployment nginx-dep --image=nginx` creates a new deployment with nginx image
`kubectl edit deployment nginx-dep` edit deployment configuration
`kubectl exec -it mongo-dep-6bb8cf99dd-jxlqt -- bin/bash` gives access to interactive terminal for provided pod
`kubectl delete deployment mongo-dep` deletes the specified deployment

## KUBECTL CONFIGURATION FILES

`kubectl apply -f ngixn-dep.yaml` executes whatever is inside the config file
