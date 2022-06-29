# PERSISTING DATA IN KUBERNETES
Kubernetes supports many types of volumes. A Pod can use any number of volume types simultaneously. Ephemeral volume types have a lifetime of a pod, but persistent volumes exist beyond the lifetime of a pod. When a pod ceases to exist, Kubernetes destroys ephemeral volumes; however, Kubernetes does not destroy persistent volumes. For any kind of volume in a given pod, data is preserved across container restarts.

# Step 1 : Created a deployment file and ran the describe command to get the region 
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/1.PNG)


# Step 2 : Created a AWS EBS(volume) and copied the id of the volume created to the deployment file.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/2.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/3.PNG)

# Step 3 : Managing Persistent Volume claims

- Run the command below to check if you already have a storageclass in your cluster`kubectl get storageclass`
 
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/4.PNG)
 
 - Create a manifest file for a PVC, and based on the gp2 storageClass a PV will be dynamically created. 
 - The nginx claim file will still be in a pending state
 
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/5.PNG)
 
 - Checking the dynamically created PV:`$ kubectl get pv`

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/5.PNG)
 
  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/6.PNG)
  
  # Step 4 : Configmap
  - Using configMaps for persistence is not something you would consider for data storage. Rather it is a way to manage configuration files and ensure they are not lost as a result of Pod replacement.
 
 - Exec into the running container and keep a copy of the index.html file somewhere. For example
```
kubectl exec -it nginx-deployment-7676f8b4d9-hv8xt -- bash
   cat /usr/share/nginx/html/index.html 
  ```
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/7.PNG)

- Listed the /usr/share/nginx/html directory

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/8.PNG)

- Listed the available configmaps. You can either use ` kubectl get cm`

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/9.PNG)

- Did a port forwading so as to access the service on the browser 

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/10.PNG)


- Confirmed on the browser the changes made.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj23/11.PNG)

  
  
