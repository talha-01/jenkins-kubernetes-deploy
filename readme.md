# Deploy a Flask application with MYSQL database on Kubernetes

## Clone the repository on the master node
```bash
cd
git clone https://github.com/talha-01/kubernetes-projects.git
```

## Create an image of the application and push Docker Hub
You can create an image using the Dockerfile or skip this step and use the image talhas/phonebook:v1 image.
```bash
cd ~/kubernetes-projects/003-flask-mysql-deploy/webserver/
docker image -t <REPO_NAME>/phonebook:v1 .
```
```bash
docker push <REPO_NAME>/phonebook:v1
```

## Create your Kubernetes objects
```bash
cd
kubectl apply -f ~/kubernetes-projects/003-flask-mysql-deploy/kubernetes/
```

## Check your secrets and cofiguration maps if they are created
```bash
kubectl get secrets
```

```bash
kubectl get configmaps
```

## Check your application deployment and database pod
```bash
kubectl get deployments
```

```bash
kubectl get pods
```

## Check your services
```bash
kubectl get services
```

## Make sure that your security group allows inbound traffic at the port 30070
As seen in the application service file, the node port is specified as port 30070. Therefore, we need to make sure that that port is allowing incoming traffic. We may need to modify either master node's and/or worker nodes' security group depending on which ip address we will be using to access our website. Let's modify the worker node's security group.

## Check your website!
Paste the worker node's IP address and add :30070 at the end.

```
http://<WORKER-NODE-IP>:30070
```




