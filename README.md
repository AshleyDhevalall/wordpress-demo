# Wordpress demo

Sample repository that demonstrates using wordpress in kind cluster.  

In this article, we will show the steps to run a cluster in single Docker container using kind. kind is a good alternative to minikube because it only uses single container of Docker.

By combining Kustomze which was integrated to Kubernetes 1.14, it is pretty straightforward to try it on your local machine.

## What is kind?
[kind](https://kind.sigs.k8s.io/) is a tool for running local Kubernetes clusters using Docker container “nodes”.

## Installing kind
To create a cluster in kind, run these commands (it takes a while)
```
$ kind create cluster
```

Then confirm “kind” cluster is available.
```
$ kind get clusters
```

To delete a cluster in kind
```
$ kind delete cluster
```

## Setting up kubectl
Also, install the latest kubernetes-cli using Homebrew or Chocolatey.
The latest Docker has Kubernetes feature but it may come with older kubectl .
Check its version by running this command.
```
$ kubectl version
```
Once kubectl and kind are ready, open bash console and run these commands.

```
$ kubectl cluster-info
```

If kind is properly set up, some information will be shown.
Now you are ready to proceed. Yay!

## Clone the repository onto your local machine
```
git clone https://github.com/AshleyDhevalall/wordpress-demo.git
```

## Apply manifest files to your cluster.
```
$ kubectl apply -k ./
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
$ kubectl get secrets
$ kubectl get pvc
$ kubectl get pods
$ kubectl get services wordpress
```
Wait until all the pods become Running status.

Then, run this command to access the service.
```
$ kubectl port-forward svc/wordpress 8080:80
```

And open http://localhost:8080/
