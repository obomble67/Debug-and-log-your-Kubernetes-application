# Debug-and-log-your-Kubernetes-application
Learn how to deploy an application Imperatively Expose the app through a Kubernetes Service Check Pod status and cluster events View application logs Debug problematic Pods
Implementation Steps

1. Deploy the Guestbook Application
Creates a Deployment object and pulls the Guestbook image into Pods.
kubectl create deployment guestbook --image=ibmcom/guestbook:v1
 

To get basic information about your pods you can use this simple command:
 

2. Describe Pod Details
But you can get much more information if you describe a specific pod, like this:
kubectl describe pod <pod name>
 
From above you can see the configuration information about the containers and the pod(labels, resource requirements, etc.), and the status information about the containers and pod (state, readiness, restart count, events, etc)

3. To get the events list of your pod, use the command:
kubectl get events [--namespace=default]
 

4. Get Application Logs
Application and system logs can help you gain a better understanding of what happened inside your cluster. You can get logs for a specific pod and if the pod has multiple containers, you can specify which container you want.

Too see your logs, you run this simple command:
kubectl logs <your-pod-name>
 

Previous Logs: You can always ask for previous logs with the --previous flag

5. Use a shell inside a running container
You can use kubectl exec to access the shell running in the container and figure out where the process is in your troublesome container, which then gives you a more comfortable way to debug your container.

In order to do so we will need to create new pod with /bin/bash:
kubectl create -f https://kubernetes.io/examples/application/shell-demo.yaml
 

Get a shell to the container:
kubectl exec -it shell-demo -- /bin/bash
 

Now list the root directory:
 

6. Debug your service
An issue that comes up frequently for new installations of Kubernetes is that the service aren't working properly, so you run your deployment and create a service but still don't get any response. In this section, we'll go over some commands that might help you figure out what's not working.

a.	 If the service you are looking for doesn’t exist, you can create it by using this command:
kubectl expose deployment <deployment-name> --type=”NodePort” --port=3000
 


b.	To see all your services, you can use a simple command like this one where we can see all pods:
kubectl get svc
 

c.	Lets try to get more information about this service:
kubectl describe service <your service name>
 
Check if you used the right NodePort to access the container and that you have endpoints.

d.	You can also get your information in json format:
kubectl get service <your-service-name> -o json
 

7. Check DNS
Some of the network problems could be caused by DNS configurations or errors. So first you’ll need to check if the DNS works correctly.

1. Use the get pods command to get your pod name:
kubectl get pods
 

2. Check the pod DNS with the following command:
kubectl exec -it <your-pod-name> -- nslookup kubernetes.default
 

3. Go inside the resolve.conf file to see if the parameters are ok:
kubectl exec <your-pod-name> cat /etc/resolv.conf
 
