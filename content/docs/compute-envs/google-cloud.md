---
title: Google Cloud Life Sciences
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Abhinav Sharma"
  - "Seqera Labs"

headline: 'Google Cloud Life Sciences Compute Environment'
description: 'Step-by-step instructions to setup Google Cloud Life Sciences for Nextflow Tower.'
menu:
  docs:
    parent: Compute Environments
    weight: 3

---
---
{{% tip "Requirements" %}}
This guide assumes you have an existing [Google Cloud Account](https://console.cloud.google.com). Sign-up for a free account [here](https://cloud.google.com/).
{{% /tip %}}

# Cloud Life Sciences

Tower provides integration to Google Cloud via the [Cloud Life Sciences API](https://cloud.google.com/life-sciences/docs/reference/rest).

The guide is split into two parts. We will begin with configuring your Google Cloud account and enabling the Google Life Sciences API. It will then guide you through creating a new Google Cloud compute environment in Tower.

## Configuration of Google Cloud

### 1. Create a new Google Cloud project or select an existing one.

Navigate to the [Google Project Selector page](https://console.cloud.google.com/projectselector2) and select an existing project or click **CREATE PROJECT**.

{{% pretty_screenshot img="/uploads/2020/09/google_create_project.png" %}}

<br>

Enter a name for your new project e.g: "tower-nf", if you are part of an organization the location will be set by default to match your organization parameters.

{{% pretty_screenshot img="/uploads/2020/12/google_new_project_name.png" %}}

<br>

### 2. Make sure Billing is enabled for the project.

At the top left of the page, in the navigation menu (**≡**) click **Billing**. You can follow the enable billing instructions [here](https://cloud.google.com/billing/docs/how-to/modify-project).

{{% pretty_screenshot img="/uploads/2020/12/google_enable_billing.png" %}}

<br>

### 3. Enable the Google Life Sciences, the Compute Engine , and the Google Cloud Storage APIs.

Open this [link](https://console.cloud.google.com/flows/enableapi?apiid=lifesciences.googleapis.com%2Ccompute.googleapis.com%2Cstorage-api.googleapis.com), to enable all **three APIs** on for your project. Select your project from the drop down menu and click **Enable**. Alternatively enable these APIs manually by selecting the project on the top bar and visiting the API pages:

**1.** [Google Cloud Life Sciences API](https://console.cloud.google.com/marketplace/product/google/lifesciences.googleapis.com)

**2.** [Compute Engine API](https://console.cloud.google.com/marketplace/product/google/compute.googleapis.com)

**3.** [Google Cloud Storage JSON API](https://console.cloud.google.com/marketplace/product/google/storage-api.googleapis.com)

{{% pretty_screenshot img="/uploads/2020/12/google_enable_apis.png" %}}

<br>

### 4. Retrieve the Compute Engine Service account for your project

Click **Go to credentials** or visit this [link](https://console.cloud.google.com/apis/credentials/wizard?api=lifesciences.googleapis.com)

{{% pretty_screenshot img="/uploads/2020/12/google_api_enabled_confirmation.png" %}}

<br>

*i)* Select the **Cloud Life Sciences API** from the dropdown menu and select the radio button **Yes, I'm using one or both** to indicate we will use the Compute Engine API. The click **What credentials do I need?**

{{% pretty_screenshot img="/uploads/2020/12/google_credential_options.png" %}}

<br>

*ii)* A second screen appears to say you do not need any further credentials. Click **Done**.

{{% pretty_screenshot img="/uploads/2020/12/google_credentials_confirmation.png" %}}

<br>

You will be redirected to the **API & Services** page and the **Credentials** section. Note a **Compute Engine default service account** has been created. **Copy** the email address as you will need this to configure **Google Storage**.

### 5. Create a new key for the compute service account

Copy and click the **Email** of the service account.

{{% pretty_screenshot img="/uploads/2020/12/google_service_account.png" %}}

<br>

*i)* Below the page select **Add key** and **Create new key**.

{{% pretty_screenshot img="/uploads/2020/12/google_service_account_create_key.png" %}}

<br>

*ii)* Select **JSON** format and press **Create**.

{{% pretty_screenshot img="/uploads/2020/12/google_service_account_create_key_json.png" %}}

<br>

A JSON key will be downloaded to your computer. This is the credentials used by Tower and you will need it to configure the Tower compute environment. In the **Service accounts** page, you can see your key is now active and you can manage it from here.

{{% pretty_screenshot img="/uploads/2020/12/google_service_account_create_key_manage.png" %}}


### 6. Create a Google Storage bucket.

In top left of the page, in the navigation menu (**≡**) click **Storage** and **Create Bucket**.

{{% pretty_screenshot img="/uploads/2020/09/google_storage.png" %}}

<br>

{{% pretty_screenshot img="/uploads/2020/12/google_create_bucket.png" %}}

<br>

### 7. Configure your Bucket

{{% warning "Bucket Naming - No underscores _ !"%}}
Do not use underscores in your bucket name and use hyphens instead.
{{% /warning %}}

*i)* Name your bucket, you will need this name to configure the Tower environment.

*ii)* Select **Region** as the **Location type** and the **Location** for your bucket. You will need the **Location** to configure the Tower environment.

*iii)* Select **Standard** as the **default storage class**.

*iv)* Select **Uniform** as the **Access control**.

{{% pretty_screenshot img="/uploads/2020/12/google_create_bucket_options.png" %}}

{{% tip %}}
The Google Cloud Life Sciences API is available in limited number of [locations](https://cloud.google.com/life-sciences/docs/concepts/locations), however, these locations are only used to store metadata about the pipeline operations. The location of the storage bucket and compute resources can be in any region.
{{% /tip %}}

### 8. Set Bucket permissions

*i)* In the **Storage** page on the **Browser** section click on the newly created storage.

{{% pretty_screenshot img="/uploads/2020/12/google_storage_browser.png" %}}

<br>

*ii)* Navigate to the **Permissions** tab and click on **+ Add**,

*iii)* Copy-paste the service account email created above into the `new members box` and add the following roles:

**Storage Admin**

**Storage Legacy Bucket Owner**

**Storage Legacy Object Owner**

**Storage Object Creator**  

{{% pretty_screenshot img="/uploads/2020/12/google_storage_roles.png" %}}

<br>

{{% star "Sweet!" %}}
You have created a project, enabled the necessary Google APIs, created a bucket and a JSON file containing required credentials. You are now ready to set up a new compute environment in Tower.
{{% /star %}}

## Tower configuration

{{% warning requirements %}}

The following guide to configure Tower assumes you have JSON keys for a configured Google Cloud account. You will also need the name and location of the **Google storage** bucket.

The [section above](#configuration-of-google-cloud) shows how to configure Google Cloud.

{{% /warning %}}

To create a new compute environment for Google Cloud in Tower follow these steps:

**1.** In the navigation bar on the upper right, choose your profile then choose **Compute environments**. Select **New Environment**.

{{% pretty_screenshot img="/uploads/2020/09/aws_new_env.png" %}}

<br>

**2.** Enter a name for this environment. For example "Google Cloud Life Sciences (europe-west2)" and select **Google Life Sciences** as the target platform.

{{% pretty_screenshot img="/uploads/2020/09/google_new_env.png" %}}

<br>

**3.** Select the **+** sign to add new credentials. Name your credentials and copy & paste the contents from the Google JSON key. If you do not have a JSON key follow [this guide](#4-enable-the-google-life-sciences-api-the-compute-engine-api-and-the-google-cloud-storage-api).

{{% pretty_screenshot img="/uploads/2020/09/google_tower_credentials.png" %}}

<br>

**4.** Select the [**Region** and **Zones**](https://cloud.google.com/compute/docs/regions-zones#available) where you'd like to deploy the workload. The **Google Storage** bucket created earlier should be accessible in the region.

{{% pretty_screenshot img="/uploads/2021/03/gcp_regions_and_zones.png" %}}

**5.** You can leave the **Location** empty and Google will run the Life Sciences API Service in the closest available location.

**6.** Enter the bucket URL in the **Pipeline work directory** e.g. *gs://my-google-bucket-name*.

{{% tip %}}
This is the name of your **Google Storage bucket** with the `gs://` prefix.
{{% /tip %}}

**7.** You can also opt-in to execute the workflow on the **Preemptible** instances to save further cost.

**8.** Optionally, options such as **Use Private Address**, **Boot disk size**, **Head Job CPUs** and **Head Job memory** could be optimized as per the requirements of the workflow as well.

**9.** Select **Create** to finalise the creation of the compute environment.

{{% pretty_screenshot img="/uploads/2021/03/google_tower_location.png" %}}

<br>

{{% star "Ace!" %}}
Time to start launching pipelines in your cloud.
{{% /star %}}

Jump to the documentation section for [Launching Pipelines](/docs/launch/overview/).
