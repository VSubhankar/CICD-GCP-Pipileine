# CICD_GCP_Pipileine
A CI/CD pipeline that automatically builds a container image from committed code, stores the image in Artifact Registry, updates a Kubernetes manifest in a Git repository, and deploys the application to Google Kubernetes Engine using that manifest.

![alt text](https://cdn.qwiklabs.com/qEN8Qxr82h1FkL8DhD5sqJmblM11i7JjhZv14OGahr0%3D)


## Setup and requirements
 - You Must have an active Google Cloud Account. (Charges May apply for usage of cloud resources so carefully check if the used resources are available for free for you or not)
 - Open your Google Cloud Dashboard
 - Click Activate Cloud Shell Activate Cloud Shell icon at the top of the Google Cloud console.
 - Create a new project

## Initialize resources
 - In Cloud Shell, set your project ID and project number. Save them as PROJECT_ID and PROJECT_NUMBER variables:
```
export PROJECT_ID=$(gcloud config get-value project)
export PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format='value(projectNumber)')
export REGION=
gcloud config set compute/region $REGION
```
 - Run the following command to enable the APIs for GKE, Cloud Build, Cloud Source Repositories and Container Analysis:

```
gcloud services enable container.googleapis.com \
    cloudbuild.googleapis.com \
    sourcerepo.googleapis.com \
    containeranalysis.googleapis.com
```
 - Create an Artifact Registry Docker repository named my-repository in the region of project to store your container images
```
 gcloud artifacts repositories create my-repository \
  --repository-format=docker \
  --location=$REGION
```
 - Create a GKE cluster to deploy the sample application of this lab:
```
gcloud container clusters create hello-cloudbuild --num-nodes 1 --region $REGION
```

## Create the Git repositories in Cloud Source Repositories
 - Follow the commands
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
gcloud source repos create hello-cloudbuild-app
gcloud source repos create hello-cloudbuild-env

```
 - Now You have to manually create 2 repositories on your configured git account
 - The first repo hello-cloudbuild-app should contain all the files mentioned in given repo
 - The second repo hello-cloudbuild-env should contain 2 chains candidate and production each of whose code is available in given repo. 

