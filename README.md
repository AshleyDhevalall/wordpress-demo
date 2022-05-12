# wordpress-demo
Sample repository that demostrates using wordpress in kind cluster.  

In this article, I will show the steps to run a cluster in single Docker container using kind.

## What is kind?
[kind](https://kind.sigs.k8s.io/) is a tool for running local Kubernetes clusters using Docker container “nodes”.

## Installing kind
Because kind is implemented in go, make sure the latest golang is installed.
To install kind, run these commands (it takes a while)

```
kind create cluster
```
Then confirm “kind” cluster is available.
```
kind get clusters
```

## Setting up kubectl
Also, install the latest kubernetes-cli using Homebrew or Chocolatey.
The latest Docker has Kubernetes feature but it may come with older kubectl .
Check its version by running this command.
```
kubectl version
```

Make sure it shows GitVersion: "v1.14.1" or above.
If you find you are running kubectlfrom Docker, try brew link or reorder path environment variable.

Once kubectl and kind are ready, open bash console and run these commands.

```
export KUBECONFIG=”$(kind get kubeconfig-path)”
kubectl cluster-info
```

If kind is properly set up, some information will be shown.
Now you are ready to proceed. Yay!

## Deploying first application
What should we deploy on the cluster? One good choice will be Wordpress because it consists of MySQL and PHP application.
Luckily, there is an official tutorial which is pretty well described. We can try most steps of it using kind cluster which we just created.

https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/

First of all, download mysql-deployment.yaml and wordpress-deployment.yaml from the page.

And create kustomization.yaml by running two cat commands.
Once those yaml files are prepared, layout them as following,

-  kustomization.yaml
-  mysql-deployment.yaml
-  wordpress-deployment.yaml

Then apply them to your cluster.
```
cd k8s-wp
kubectl apply -k ./
```  

You will see outputs like this if the command succeeded.
```
secret/mysql-pass-7tt4f27774 created
service/wordpress-mysql created
service/wordpress created
deployment.apps/wordpress-mysql created
deployment.apps/wordpress created
persistentvolumeclaim/mysql-pv-claim created
persistentvolumeclaim/wp-pv-claim created
```

Let’s check cluster’s status by typing these commands:
```
kubectl get secrets
kubectl get pvc
kubectl get pods
kubectl get services wordpress
```

Wait until all the pods become Running status.
Then, run this command to access the service.
```
kubectl port-forward svc/wordpress 8080:80
```

And open http://localhost:8080/


Voila!
If you want to inspect database, check your pods, run a command like this and open your client app.

kubectl port-forward wordpress-mysql-bc9864c58-ffh4c 3306:3306
Conclusion
kind is a good alternative to minikube because it only uses single container of Docker.
By combining Kustomze which was integrated to Kubernetes 1.14, it is pretty straightforward to try it on your local machine.
