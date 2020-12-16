  ---
title: Google GKE
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Jordi Deu-Pons"
  - "Paolo Di Tommaso"
  - "Alain Coletta"
  - "Seqera Labs"

headline: 'Google GKE Compute environments'
description: 'Step-by-step instructions to set up a Tower compute environment for Google GKE cluster'
menu:
  docs:
    parent: Compute Environments
    weight: 6

---
## Overview

[Google GKE](https://cloud.google.com/kubernetes-engine) is managed Kubernetes cluster that allows to run containerised workloads in the Google cloud at scale.

Nextflow Tower has a native support for Google GKE cluster and streamline the deployment
of Nextflow pipelines in such environment.


## Requirement

You need to have GKE cluster up and running. Make sure you have followed
the steps in the [Cluster preparation](https://github.com/seqeralabs/nf-tower-k8s) guide to create the cluster resources required
by Nextflow Tower.


## Compute environment setup  

**1.** In the navigation bar on the upper right, choose your account name then choose **Compute environments** and select **New Environment**.

{{% pretty_screenshot img="/uploads/2020/09/aws_new_env.png" %}}

</br>

**2.** Enter a descriptive name for this environment. For example, *My GKE* and select **Google GKE** as the target platform.

{{% pretty_screenshot img="/uploads/2020/12/gke_new_env.png" %}}

**3.** Select your Google Cloud credentials. The credentials are needed to indentify the user that will access the GKE cluster.

**4.** Select the *Location* where the GKE cluster is located. 

{{% warning "Regional and zonal clusters" %}}
GKE clusters can be either *regional* or *zonal*. For example, the `us-west1` identify the United States West-Coast region and it has three zones: `us-west1-a`, `us-west1-b`, and `us-west1-c`.

<br>
Tower self-completion only shows regions. You can manually edit this field if your cluster is in a zone depending if your GKE cluster was created as regional or zonal.
<br>
{{% pretty_screenshot img="/uploads/2020/12/gke_regions.png" %}}

{{% /warning %}}

**5.** The field **Cluster name** lists all GKE cluster available in the selectd location. Choose the one you want to use to deploy the Nextflow execution.

**6.** Speicify Kubernetes **Namespace** that should be used to deployment the pipeline execution. 

If you have followed the example in the [cluster preparation](https://github.com/seqeralabs/nf-tower-k8s/blob/master/cluster-preparation.md#2-service-account--role-creation) guide this field should be `tower-nf`.

**7.** Specify the Kubernetes **Head service account** that will be used to grant permissions to Tower to deploy the pods executions and related. 

If you have followed the [cluster preparation](https://github.com/seqeralabs/nf-tower-k8s/blob/master/cluster-preparation.md#2-service-account--role-creation) guide this field should be `tower-launcher-sa`. 

**8.** The **Storage claim** field allows you to specify the storage Nextflow should use as 
scratch file system for the pipeline exection. 

Following the example in the [cluster preparation](https://github.com/seqeralabs/nf-tower-k8s/blob/master/cluster-preparation.md#3-storage-configuration) guide this should be `tower-scratch`. Change accordingly if you are using a different persistent storage. 

## Advanced options

The following parameters are available:

**1.** The **Storage mount path** defines the file system path where the Storage claim is mount. Default: `/scratch`

**2.** The **Work directory** field defines the file system path used as working directory by the Nextflow pipelines. It must be the same or a subdirectory of the *Storage mount path* at the previous point. Defualt: the same as *Storage mount path*.

**3.** The  **Compute service account** field allows you specify the Kubernetes *service account* that the pipeline jobs should use. Default is the `default` service account in your Kubernetes cluster.
