# DEPLOYING APPLICATIONS INTO KUBERNETES CLUSTER

This project demonstrates how containerised applications are deployed as pods in Kubernetes and how to access the application from the browser.

# Step 1 : Created an AWS EKS cluster using the github terraform code repository for aws provider
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/eks.PNG)

# Step 2 : Creating A Pod For The Nginx Application
- Created a nginx-pod. yaml file and ran the command `kubectl apply -f nginx-pod.yaml`
- Did experience major errors due to the yaml indentation.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/1.PNG)

- Running the following commands to inspect the setup:
```
$ kubectl get pod 

$ kubectl get pod nginx-pod -o wide
```

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/2.PNG)


# Step 3 : Accessing The Nginx Application Through A Browser
-Running the kubectl command to run the container that has curl software in it as a pod:$ `kubectl run curl --image=dareyregistry/curl -i --tty`
- Running curl command and pointing it to the IP address of the Nginx Pod

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/3.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/4.PNG)

- Created a service for the nginx pod by applying the manifest file:`$ kubectl apply -f nginx-service.yaml`

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/5.PNG)

- Because there is no way the service will be able to select the actual Pod it is meant to route traffic to. If there are hundreds of Pods running, there must be a way to ensure that the service only forwards requests to the specific Pod it is intended for.
- Updated the nginx-pod  and ran the port forwading command `kubectl  port-forward svc/nginx-service 8089:80`

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/6.PNG)

- Accessed the service via the web browser by entering localhost:8089

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/7.PNG)

# Step 4 : Creating A Replica Set

- Created a rs.yaml manifest and ran the command `kubectl apply -f rs.yaml`

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/8.PNG)

- Inspected the setup 
 `$ kubectl get pods

$ kubectl get rs -o wide
`

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/9.PNG)

- Scaled the replicaset to 5 `kubectl scale --replicas 5 replicaset nginx-rs`

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/10.PNG)

- Scaled the replicaset to 3 
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/11.PNG)


- Edited the rs file to include environments such as dev,qa,prod and application tier such as frontend etc.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/12.PNG)

# Step 5: Using AWS Loadbalancer to access my K8s Service
- Updated the service `kubectl apply -f nginx-service.yaml` and ran the command to get the newly created service `kubectl get service nginx-service`

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/13.PNG)

- Confirmed the created resource on my AWS console
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/14.PNG)

- Updated the service to get the hostname for the load balancer.
 
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/15.PNG)
 
 - Accessed the nginx service on my web browser using the loadbalancer address 
 
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/16.PNG)
