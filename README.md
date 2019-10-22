# Introduction to Appsody with OpenShift / Minishift

Cloud Native application development reflects a transition from earlier platform environments based on Java EE container systems and other runtimes. In the Cloud Native developer world, there are a plethroa of choices to be made, and a broad open source ecosystem of tooling and frameworks to choose from. Additionally, with the organizational culture and operational changes introduced by the DevOps model, a change in responsibilities is hitting development and operations teams compared to previous eras.

Successfully navigating the transition from monolith applications running on servers and virtual machines to microservices running in containers on [Kubernetes](https://kubernetes.io) by these teams is both complicated and essential. The [Kabanero](https://kabanero.io) project is a collection of open source tools that run in a Kubernetes envrionment to help developers in this transition. This lab focuses specifically on the [Appsody](https://appsody.dev) component of the project that provides standardization of runtimes and frameworks for developers to use in creating Cloud Native applications. Appsody does this through the construct of a **stack**, which combines all that a developer needs in a container for a programming language runtime including frameworks for building and testing.

In this guide, you will see how to install and configure a local copy of [Minishift](https://www.okd.io/minishift/), an open source version of the OpenShift container platform that is optimized for smaller size and development use cases. You will add to this cluster support for the [Operator Framework](https://github.com/operator-framework/) and the [Appsody Operator](https://github.com/appsody/appsody-operator).

**NOTE** If you have previously completed the [Guestbook with OpenShift](https://github.com/timroster/guestbook-openshift) lab, you may skip the Minishift installation and pick up at the Appsody Operator installation step.

With the initial setup complete, you will see how to use Appsody to create a starting application, run it in a container and then perform local loop development and updates to the application. After you have tested the application locally, it will be time to deploy the application to the Minishift cluster using the Appsody operator.

This application can also be deployed to an existing OpenShift 3.9 or higher cluster by skipping the Minishift cluster setup steps. The application, including IBM Cloud operator may also be deployed on an OpenShift 4.x cluster by skipping the Minishift cluster setup and the OLM / operator marketplace setup steps (support for operator marketplace ships in OpenShift 4.x).

## Prerequisites

* Workstation (MacOS, Linux or Windows 10) with hypervisor support, either native to the OS or through VirtualBox.
* For Windows 10, support for a bash shell, for example either by setting up [git bash](https://gitforwindows.org/) or the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
* Internet connectivity for downloading Minishift and connecting to the IBM Cloud.
* Docker: [Docker Desktop for Mac and Windows](https://www.docker.com/products/docker-desktop) or Docker for your Linux system, for running Appsody

## Time required

Expect to spend about 45 minutes to create the cluster, deploy the operator framework and Appsody operator and then explore a sample application with Appsody. Additional time may be needed for configuring your workstation hypervisor configuration.

## Getting started

Begin by creating a local clone of this repository on your workstation with either git clone:

```text
git clone https://github.com/timroster/appsody-openshift.git
```

Or by downloading a zip file of the repository and unpacking locally.

## Steps to install Minishift, configure Operator Framework support, create the IBM Cloud operator and deploy the guestbook application

* [Set up Minishift on your system](workshop/minishift.md)
* [Install Operator Framework and Appsody Operator](workshop/appsody-operator.md)
* [Appsody 101](workshop/appsody101.md)
* [Deploying appsody apps to Kubernetes](workshop/appsody-deploy.md)

## Summary and next steps

Visit the IBM Developer [OpenShift on IBM Cloud collection](https://developer.ibm.com/collections/openshift-on-ibm/) to learn more about using OpenShift on IBM Cloud. For additional step-by-step learning activities related to OpenShift check out [learn.openshift.com](https://learn.openshift.com)
