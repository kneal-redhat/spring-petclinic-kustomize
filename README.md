# Pragmatic GitOps: Part 1 - Kustomize

## Introduction


There is also the final "base" and "overlays" directories, so you can see what the finished product should look like.

## Deploying the "petclinic-demo" Project

Login to your OpenShift cluster (4.5+) with the `oc` command line tool and run the following:

```
$ git clone https://github.com/kneal-redhat/spring-petclinic-kustomize.git
$ cd spring-petclinic-kustomize
$ oc new-project petclinic-demo
$ oc apply -f demo
```

This will spin up the same app and database.  If the petclinic app starts before the MySQL database initializes, you will have to scale the app down to zero, then back up again (or just kill the pod) in order for the app to start properly once the database is ready.

## Deploying the DEV and PROD Environments

If you just want to deploy the resulting DEV and PROD environments, then first login to your OpenShift cluster with the `oc` command line tool.  You should be logged in as a user than can create new namespaces.  Then, run the following commands.  You can use either `kubectl` of `oc` to run the `apply` commands.  

```
$ git clone https://github.com/kneal-redhat/spring-petclinic-kustomize.git
$ cd spring-petclinic-kustomize
$ kubectl apply -k overlays/dev
$ kubectl apply -k overlays/prod
```

This will create the `petclinic-dev` and `petclinic-prod` namespaces and deploy the app in both.

Don't want to have to clone this repository, make sure you have cli access to your OpenShift cluster, run the following commands (you can also substitute `oc` for `kubectl` if you like):

```
$ kubectl apply -k https://github.com/kneal-redhat/spring-petclinic-kustomize/overlays/dev?ref=main
$ kubectl apply -k https://github.com/kneal-redhat/spring-petclinic-kustomize/overlays/prod?ref=main
```


## Next Steps

Use [Argo CD](https://argoproj.github.io/argo-cd/) to deploy the application and provide lifecycle capabilities.