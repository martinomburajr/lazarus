# Lazarus
Lazarus is an application deploys a Google Cloud Function and Scheduler that allows you to resurrect a set of selected Compute Engine Preemptible Virtual Machines.

## How It Works
The script is written in Golang and uses the Google Cloud Platform API to deploy a Cloud Scheduler with a specified `crontab` as well as a Google Cloud Function that runs a script that observes a defined set of VMs and polls their status. 

Instances in the **`STOPPED`** **`SUSPENDED`** and **`TERMINATED`** states will be **resurrected** if found to be stopped.

## Deployment Guide

### 1. Installation
You must have the gcloud SDK installed so that the application can retrieve your identity from the gcloud environment.
##### 1.1 Clone the Repo

Download the .zip file or clone the repository. If you have Go installed on your machine, run go install in the directory of the zip file.


### 2. Running the Application
    laz deploy -n=mylazarus-p=<your-project-id> -s=<service-account-name> -cron="* * * * *" -vms="[{vm: myv1, zone: <gcp-zone>}, {vm: myv2, zone: <gcp-zone>]
  
    Flags
    -n --name = [name]: This is a name to identify your lazarus components. All components are prefixed with `lazarus-` for ease of search. e.g. `lazarus-cloud-scheduler` & `lazarus-cloud-function`.
    -p --project = [project-id]: This is your GCP project ID.
    -s --serviceaccount = [serviceaccount]: This is an already created service account within *proj* that you specified
    -c --cron = [crontab]: This is the specified crontab that polls the cloud function. A recommended number could be "\*/10 * * * \*" which is a cron for every 10 minutes. You **must** use quotes to submit your cron
    -v --vms = [virtualmachines]: This is an array of json objects that list your VMs as well as their respective zones.
    -r --region = [region]: This is the region to deploy the cloud function and cloud scheduler.
  
#### 2.2 Example of code

    laz deploy -n=vm-raiser -p=my-gcp-proj -s=mylazarussa@my-gcp-proj.iam.gserviceaccount.com -cron="\*/10 * * * \*" -vms="[{vm: myv1, zone: europe-west1-d}, {vm: myv2, zone: us-central-1a>]" -r=us-central1
  
  ##### Description
  This uses the lazarus application you installed to deploy a lazarus component named **`vm-raiser`** under the GCP project **`my-gcp-proj`** using the **`mylazarussa@my-gcp-proj.iam.gserviceaccount.com`** service account you created in **`my-gcp-proj`**. The scheduler uses the **`"\*/10 * * * \*"`** i.e polls the VMs status every 10 minutes. The Cloud Function watches **`myvm1`** in **`europe-west1-d`** and **`myvm2`** in **`us-central1-a`**. The Cloud Scheduler and Cloud Functions would be deployed to the **`us-central1`** region.
  
### 3. Contributors
Martin Ombura Jr. <info@martinomburajr.com>

### 4. Contribute
If you feel led to contribute, please feel free to create an issue then file an associated pull-request. Please detail your issues to allow transparency.

###

