# kubevirt-gitops

This repository is an example of managing KubeVirt virtual machines
and datavolumes using a GitOps workflow, specifically using ArgoCD
as provided by OpenShift GitOps.

Where possible, namespaces and resource names have been left variable
for portability, but since this is in support of an OpenShift blog,
the default leans to OpenShift products and namespaces.

## Get Started

Under the [setup](setup) directory, there are a number of scripts to
help in an OpenShift GitOps deployment.

To run everything, use [`install.sh`](setup/install.sh) without any arguments.
This script will:

  - Discover the ArgoCD namespace and configuration CR names.
  - Install health check scripts that ArgoCD uses to better track status of custom resources.

More in-depth descriptions may be found in the [README](setup/)

## Tested on OpenShift 4.14

TO-DO
* fix user password in autounattend.xml

## Quick Start
**Deploy OpenShift Resources**
```
oc apply -k apps/overlays/openshift
```

**Deploy Windows Application Workload**
```
oc apply -k apps/overlays/windows
```


## Application Directories

Other directories in this repository are designed to support Applications in ArgoCD. Each of these directories
contains a [kustomization.yaml](https://kubectl.docs.kubernetes.io/references/kustomize/glossary/#kustomization)
and an [application.yaml](https://argo-cd.readthedocs.io/en/stable/getting_started/#6-create-an-application-from-a-git-repository).

### KubeVirt

Under the [kubevirt](kubevirt) directory is an ArgoCD application and manifests
to install KubeVirt on a Kubernetes cluster.

### OpenShift Virtualization

Under [virtualization](virtualization) are manifests to create an OpenShift
Virtualization installation using the OpenShift Operator Lifecycle Manager
(OLM).

### DataVolumes

The [datavolumes](datavolumes) directory contains DataVolume manifests which
will download specific cloud images of Fedora and CentOS 8 Stream for use as
boot sources for OpenShift Virtualization's included OS templates.

### Virtual Machines

The [vms](vms) directory works with the DataVolumes provided in
[datavolumes](datavolumes) to create a Fedora and CentOS VM.

### Microsoft Windows 2019 Server Install from ISO

The [win2k19](win2k19) directory holds an application and manifests for
creating a boot source image of a Microsoft Windows 2019 Server VM.

Link: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019
This application requires a MS Windows 2019 Server installation ISO be
downloaded and shared using a local web service accessible only to the cluster.
The URL for the ISO is found in [install-iso.yaml](win2k19/install-iso.yaml)
and should be changed for the local environment. Alternatively, the ISO may be
manually uploaded to the kubevirt-gitops namespace as a DV, and the
install-iso.yaml entry removed from
[kustomization.yaml](win2k19/kustomization.yaml).

Installation scripts can be found in [config](win2k19/config), including a job
script which runs once the ISO is available, creates the VM, waits for it to
shut down, and then copies the installed root disk into the boot source
namespace. A short script is provided at
[regen-configmap.sh](win2k19/regen-configmap.sh) that updates the ConfigMap
supplied by the repo. If any changes are made, run the script to regenerate the
ConfigMap, and make sure to check in any changes for ArgoCD to see them.


## To enable RHEL VMs to auto register see below 
`Settings-> Guest Management -> Auto Register Automatic subscription of new RHEL VirtualMachines`
![20240723165919](https://i.imgur.com/0OphjEq.png)

## Links:
* [Using Red Hat Advanced Cluster Management and OpenShift GitOps to manage OpenShift Virtualization
](https://www.redhat.com/en/blog/using-red-hat-advanced-cluster-management-and-openshift-gitops-to-manage-openshift-virtualization)