# README: Setting Up a GPU-Enabled Docker Environment for Machine Learning

This README provides a step-by-step guide to setting up a GPU-enabled environment in Google Cloud, building a Docker container for machine learning purposes, and deploying it.

## Table of Contents
1. [Requesting GPU Quota from Google Cloud](#requesting-gpu-quota-from-google-cloud)
2. [Creating a Dockerfile for Training](#creating-a-dockerfile-for-training)
3. [Securely Passing Credentials to a Remote Docker Container](#securely-passing-credentials-to-a-remote-docker-container)
4. [Setting Up Continuous Deployment](#setting-up-continuous-deployment)
5. [Creating a Virtual Instance with GPU](#creating-a-virtual-instance-with-gpu)

## Requesting GPU Quota from Google Cloud
- Navigate to [Google Cloud Quotas Page](https://console.cloud.google.com/iam-admin/quotas).
  ![Google Cloud Quotas Page](https://prnt.sc/R9Ygo1PD8fwu)
- Search for "T4 GPU" and select "Commited NVIDIA T4 GPUs".
  ![T4 GPU Selection](https://prnt.sc/Fv0T_UECJxoJ)
- Choose all European regions and click 'Edit Quotas'.
- In the request form, specify you need 1 GPU and provide a brief description of your project.

## Creating a Dockerfile for Training
- Name the Dockerfile as `Dockerfile.training`.
- The key difference between this Dockerfile and the API one is...
  (Please provide additional details or instructions here.)

## Securely Passing Credentials to a Remote Docker Container
- Visit [Prefect Cloud](https://app.prefect.cloud/).
  ![Prefect Cloud](https://prnt.sc/sVd-LbKRusNQ)
- Navigate to 'Blocks' and choose 'Add New Block'.
  ![Add New Block](https://prnt.sc/ctnRAEvNkvDP)
- Search for 'GCP' and fill in the necessary details.
  ![GCP Search](https://prnt.sc/BWqhDm56w9Pv)
- Include `prefect_gcp` in your `requirements.txt`.
- Load credentials in your Python script as follows:
    ```python
    from prefect_gcp import GcpCredentials

    async def load_google_credentials():
        gcp_credentials = await GcpCredentials.load("facetallygcp")
        return gcp_credentials.get_credentials_from_service_account()
    ```

## Setting Up Continuous Deployment
- Access [Cloud Build in Google Cloud](https://console.cloud.google.com/cloud-build).
- Connect your repository and create a build trigger.
- Configure the image name and other settings as needed.
- Run the build and monitor the process in the 'History' tab.

## Creating a Virtual Instance with GPU
- Go to [Google Cloud VM Instances](https://console.cloud.google.com/compute/instances).
  ![Google Cloud VM Instances](https://prnt.sc/Kl4Om05x3bBs)
- Create a new instance, ensuring to select 'Deploy Container' and enter the Docker image name.
  ![Deploy Container](https://prnt.sc/5yz35u-diI7h)
- Set environmental variables as required.
  ![Environmental Variables](https://prnt.sc/OM8cm9Fem34Z)
- Choose the appropriate boot disk size and other configurations.

Remember to use the command line for similar operations if preferred. Detailed instructions are provided.

### Note
Ensure to structure and elaborate each section according to the specific steps and details of your project.
