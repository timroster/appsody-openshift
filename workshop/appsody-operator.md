# Installing the Operator Framework and Appsody Operator

The Operator Framework provides support for Kubernetes-native extensions to manage custom resource types through operators. Many operators are available through [operatorhub.io](https://operatorhub.io/), including the Appsody operator. The Appsody operator automates the deployments of applications into a Kubernetes cluster. The instructions in this guide are adapted from the IBM Developer tutorial [Simplify the lifecycle management process for your Kubernetes apps with operators](https://developer.ibm.com/tutorials/simplify-lifecycle-management-kubernetes-openshift-ibm-cloud-operator/).

The steps in this guide require execution by a user with cluster-admin role. If you are using Minishift, be sure to enable the `admin-user` addon and then log in to the `oc` cli using this user:

`oc login -u admin -p the-admin-passwd`

## Installing the Operator Framework

Begin by applying the Operator Lifecycle Manager (OLM) release to your cluster:

```text
oc apply -f https://github.com/operator-framework/operator-lifecycle-manager/releases/download/0.10.0/crds.yaml
oc apply -f https://github.com/operator-framework/operator-lifecycle-manager/releases/download/0.10.0/olm.yaml
```

Before continuing, check to ensure that the pods in the `olm` namespace have started. This can take a few minutes for the container images to be pulled and the pods to start.

```text
oc get pods --namespace olm
NAME                                READY     STATUS    RESTARTS   AGE
catalog-operator-6bb8ffd7c5-4jwwz   1/1       Running   0          2m
olm-operator-78ff5d69cf-2ssbs       1/1       Running   0          2m
olm-operators-ts4h4                 1/1       Running   0          1m
operatorhubio-catalog-q2kwj         1/1       Running   0          1m
packageserver-567b8d96b7-d75vp      1/1       Running   0          11s
packageserver-567b8d96b7-hlx8g      0/1       Running   0          11s
```

Next, to install the Marketplace Operator (allows for adding operators from the operator marketplace) perform these steps from a folder/directory outside of the `guestbook-openshift` repository folder:

```text
git clone https://github.com/operator-framework/operator-marketplace.git
oc apply -f operator-marketplace/deploy/upstream/
```

Now, configure a namespace for the marketplace operators

```text
oc apply -f - <<END
apiVersion: operators.coreos.com/v1alpha2
kind: OperatorGroup
metadata:
  name: marketplace-operators
  namespace: marketplace
END
```

At this point, you should be able to install any operator from the [operatorhub.io](https://operatorhub.io/) marketplace.

## Installing the Appsody operator

Continue with these steps with the same user logged into the cluster. With the OLM framework and marketplace support installed, it's time to install the Appsody operator. This operator will install into the `operators` namespace and watch all namespaces for new resources of kind `AppsodyApplication`. When one of these resources is created, the operator will create either a new application deployment or Knative service corresponding to the deployment yaml.

### Install the Appsody operator from the operator marketplace

The operator marketplace catalog provides a URL for the resources to install for each operator. Install the Appsody Operator with the following command:

```text
oc create -f https://operatorhub.io/install/appsody-operator.yaml
```

Check that the pod for the Appsody operator is running:

```bash
$ oc get pods --namespace operators
NAME                                 READY     STATUS    RESTARTS   AGE
appsody-operator-6b6887fdb-pzfl2     1/1       Running   0          17s
```

> note you may see an IBM Cloud Operator too if you are using a MiniShift cluster from the previous webcast

## Summary

Your cluster now has the Appsody operator installed. This operator is able to configure a new kind in the cluster, a **AppsodyApplication**. The **AppsodyApplication** is a deployment of an application component based on an Appsody stack and can run either as an always-on container or as a serverless container that is automatically started when a request hits. For more details about the Appsody operator and options see the [user guide](https://github.com/appsody/appsody-operator/blob/master/doc/user-guide.md)

Continue to [get started with Appsody](appsody101.md)
