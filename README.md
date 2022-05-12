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
Creating cluster "kind" ...
 • Ensuring node image (kindest/node:v1.23.4) 🖼  ...
 ✓ Ensuring node image (kindest/node:v1.23.4) 🖼
 • Preparing nodes 📦   ...
 ✓ Preparing nodes 📦
 • Writing configuration 📜  ...
 ✓ Writing configuration 📜
 • Starting control-plane 🕹️  ...
 ✓ Starting control-plane 🕹️
 • Installing CNI 🔌  ...
 ✓ Installing CNI 🔌
 • Installing StorageClass 💾  ...
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
```

Then confirm “kind” cluster is available.
```
$ kind get clusters
kind
```

To delete a cluster in kind
```
$ kind delete cluster
Deleting cluster "kind" ...
```

## Setting up kubectl
Also, install the latest kubernetes-cli using Homebrew or Chocolatey.
The latest Docker has Kubernetes feature but it may come with older kubectl .
Check its version by running this command.
```
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.0", GitCommit:"ab69524f795c42094a6630298ff53f3c3ebab7f4", GitTreeState:"clean", BuildDate:"2021-12-07T18:16:20Z", GoVersion:"go1.17.3", Compiler:"gc", Platform:"windows/amd64"}
Server Version: version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.4", GitCommit:"e6c093d87ea4cbb530a7b2ae91e54c0842d8308a", GitTreeState:"clean", BuildDate:"2022-03-06T21:32:53Z", GoVersion:"go1.17.7", Compiler:"gc", Platform:"linux/amd64"}
```
Once kubectl and kind are ready, open bash console and run these commands.

```
$ kubectl cluster-info
Kubernetes control plane is running at https://127.0.0.1:59593
CoreDNS is running at https://127.0.0.1:59593/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
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
