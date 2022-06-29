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

# Step 6: Creating Deployment

A Deployment is another layer above ReplicaSets and Pods, newer and more advanced level concept than ReplicaSets. It manages the deployment of ReplicaSets and allows for easy updating of a ReplicaSet as well as the ability to roll back to a previous version of deployment. It is declarative and can be used for rolling updates of micro-services, ensuring there is no downtime.

- Deleting the ReplicaSet that was created before: `$ kubectl delete rs nginx-rs`
- Creating deployment manifest file called deployment.yaml and applying it:`$ kubectl apply -f deployment.yaml`

- Inspecting the setup: ` kubectl get deployments`

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/17.PNG)
 
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/18.PNG)

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/19.PNG)
 
 
- To exec into one of the pods:`kubectl exec nginx-deployment-6fdcffd8fc-277g5 -i -t -- bash`
  
  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/20.PNG)
  

# Step 7: Persisting data for pods

- Scale the Pods down to 1 replica.
  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/21.PNG)
  
- Installed vim so that you can edit the file

``
apt-get update
apt-get install vim
``
- Update the content of the file and add the code below /usr/share/nginx/html/index.html
```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to DAREY.IO!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to DAREY.IO!</h1>
<p>I love experiencing Kubernetes</p>

<p>Learning by doing is absolutely the best strategy at 
<a href="https://darey.io/">www.darey.io</a>.<br/>
for skills acquisition
<a href="https://darey.io/">www.darey.io</a>.</p>

<p><em>Thank you for learning from DAREY.IO</em></p>
</body>
</html>
```

- Checked the browser.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/22.PNG)

- Deleted the only running Pod
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/24.PNG)

- Refresh the web page – You will see that the content you saved in the container is no longer there. That is because Pods do not store data when they are being recreated – that is why they are called ephemeral or stateless.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj22/25.PNG)
