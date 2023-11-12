# Minikube and Kubernetes


## Prerequisites

- Ensure that Minikube is installed on your machine.

## Creating a Kubernetes Cluster with Minikube

You can create a Kubernetes cluster using a virtual machine manager like VirtualBox (or VMware, Hyper-V...) by specifying the number of CPUs and memory capacity:

   ```bash
   minikube start --vm-driver virtualbox --cpus 4 --memory 3192
   ```

## Accessing the Kubernetes Dashboard  

To access the Kubernetes dashboard, use the following command:

   ```bash
   minikube dashboard
   ```
If you encounter an error, it may require the metrics-server module. To enable all features, run:
   ```bash
   minikube addons enable metrics-server
   ```
Then rerun the command to access the dashboard.

## Obtaining the Minikube IP Address 
This command returns the IP address of the virtual machine hosting your local Kubernetes cluster.
   ```bash
   minikube ip
   ```
## Creating a Deployment
This command creates a deployment named "kubernetes-bootcamp" that uses the specified Docker image to create pods.
   ```bash
   kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
   ```
## Executing Commands in Pods
You can execute commands in pods just like you would with Docker, for example : 
   ```bash
   kubectl exec -it pod-name -- bash
   ```
## Exposing the App
By default, a pod is only visible to other pods within the same Kubernetes cluster in the same private network. 
To expose the app externally : 
   ```bash
   kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080  
   ```
   This is equivalent to creating a service :
   ```bash
   kubectl get services
   ```

## Accessing the Application
Access the application via the cluster's IP address and port:

- Cluster address: minikube ip   
- Port: specified when exposing the service  
- For example: http://cluster-address:port

## Scaling a Deployment
Scaling a deployment is done with the command:
```bash
kubectl scale deployments/kubernetes-bootcamp --replicas=4
```
## Service as a Load Balancer
The service acts as a load balancer, redirecting traffic to different pods.

Test with:
```bash
curl $(minikube ip):port
```
or enter the address in your browser.
## Updating Docker Image
To update the Docker image of a deployment:
```bash
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```
## Creating a Deployment and Service in a Single YAML File
You can automate this manual process by using a YAML file that contains all the necessary configurations.   
 First, eliminate the manual steps you performed
```bash
kubectl delete deployments --all
kubectl delete services --all
```
And then, you clone this repository that includes the YAML file and execute the following command:
```bash
kubectl apply -f k8s.yml
``` 




