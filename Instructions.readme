1. We need to request permission from google to get the GPU
a. Open this page [https://console.cloud.google.com/iam-admin/quotas]
https://prnt.sc/R9Ygo1PD8fwu
In this field you have to tap in:
T4 GPU - you will see the first option pop up:
Commited NVIDIA T4 GPUs
Select it.
You will see a list of regions - select all european ones and press edit quotas.
https://prnt.sc/Fv0T_UECJxoJ

In the popup window insert 1 - we are asking for 1 gpu and fill in description, that you work on personal project in bootcamp and you need it (maybe small description of project).
Then we will have to wait 20-30 minutes for the approval.

While waiting we should start working on the Dockerfile to build our image where the model will be trained.

Create dockerfile with this name - Dockerfile.training (to separate from the API one).
Difference between two - chapt gpt fill in this gap.

What is a safe way to pass credentials into remote docker container?
One of the ways is using Prefect.
1) go to the page https://app.prefect.cloud/
2) select blocks on the left https://prnt.sc/sVd-LbKRusNQ
3) Select add new block https://prnt.sc/ctnRAEvNkvDP
4) Search for gcp - https://prnt.sc/BWqhDm56w9Pv press add
5) Fill in contents of the json https://prnt.sc/Dc7LcSmrrkF_

Add prefect_gcp to your requirements.txt file, run pip install -e . or pip install -r requirements.txt to install it.

Now you can load your credentials like that:

```python
from prefect_gcp import GcpCredentials

# Load your GCP credentials block
gcp_credentials = await GcpCredentials.load("facetallygcp")
credentials =gcp_credentials.get_credentials_from_service_account()

from google.cloud import storage

client = storage.Client(credentials=credentials)
```

Only small moment you have to use async to get it into function:
async def load_google_credentials():
    # Asynchronously load the GCP credentials
    gcp_credentials = await GcpCredentials.load("facetallygcp")
    return gcp_credentials.get_credentials_from_service_account()


# SIDE note -its much faster to store data as zipped in cloud and unpuck zip when loading


run prefect config view to get api

# Go to artifact registry https://console.cloud.google.com/artifacts
Here are all of our docker images stored
select create repository https://prnt.sc/AtGtMY-EhgAo
fill in with your project name https://prnt.sc/Pgyf460XOpYe select europe-southwest1 (Madrid)	as region and press create


in your terminal run gcloud auth configure-docker europe-southwest1-docker.pkg.dev IF YOU HAVE NOT DONE THIS DURING CHALLANGES



Setup continious deployment - building this images can take out lots of space and resourses of you pc, so ideally we want to always build in cloud.
How to set this up:
Go to cloud build page: https://console.cloud.google.com/cloud-build press enable if it is not enabled yey
select repositories and then connect repositories https://prnt.sc/voTxcDWNo7tM
select repo https://prnt.sc/tfzwxyIxdGpE
press create trigger

things you have to change:
https://prnt.sc/RUP-IZq_p2A5

exmaple of Image name
europe-southwest1-docker.pkg.dev/$PROJECT_ID/facetally/facetally_image:prod


list of triggers will open - press run https://prnt.sc/LQxEylRDKpYR

select History on the left and then you can press on latest build in progress to checkout all the logs https://prnt.sc/K55YoKuI63DQ
if there will be errors update the image and just push to github to the master branch https://prnt.sc/lS_Dmg4exE_a

After process is finished your image will be automatically pushed to Artifact Registry, where all of them are stored, now

Now we need to create instance
we need to go to create an virtual instance with gpu
https://console.cloud.google.com/compute/instances
Press create instance
things you have to update:
1) https://prnt.sc/Kl4Om05x3bBs
2) Select deploy container https://prnt.sc/5yz35u-diI7h
Insert name of docker image like this europe-southwest1-docker.pkg.dev/wagon-bootcamp-355610/facetally/facetally_image
fill in all the enviromental vars as well https://prnt.sc/OM8cm9Fem34Z

press change boot disk https://prnt.sc/aySQoyhEKLM-

change size of instance to 50/100gb https://prnt.sc/muafxB12lCpk


    Command line to do the same in CLI

gcloud compute instances create-with-container facetally --project=wagon-bootcamp-355610 --zone=europe-central2-b --machine-type=n1-standard-8 --network-interface=network-tier=PREMIUM,subnet=default --maintenance-policy=TERMINATE --provisioning-model=STANDARD --service-account=1047329738761-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --accelerator=count=1,type=nvidia-tesla-t4 --image=projects/cos-cloud/global/images/cos-stable-109-17800-66-27 --boot-disk-size=100GB --boot-disk-type=pd-standard --boot-disk-device-name=facetally --container-image=europe-southwest1-docker.pkg.dev/wagon-bootcamp-355610/facetally/facetally_image --container-restart-policy=always --container-env=SPLIT_RATIO=0.2,BATCH_SIZE=16,LEARNING_RATE=0.001,EPOCH=1000,GLOBAL_CLIPNORM=10.0,GCP_PROJECT=imposing-league-401513,GCP_REGION=europe-west-1,BUCKET_NAME=facetally_data,BOX_FORMAT=rel_xyxy,BACKBONE_SIZE=s,PREFECT_API_URL=https://api.prefect.cloud/api/accounts/bd80874a-c8d0-41a5-96cd-263951d5497f/workspaces/e4c32e6b-ba09-4f30-92a5-068be294085a,PREFECT_API_KEY=pnu_wRsCvORCKSVSxS8fBycGqWbIECnKsS1n95zt --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --labels=goog-ec-src=vm_add-gcloud,container-vm=cos-stable-109-17800-66-27
