# PRE-REQS
Those are the pre-requisites for runnign the following Terraform execution with Cloud Build demo:
- https://www.loom.com/share/80ae6ee6d8d645bc967461cafbf0d119

## Enable APIs
- gcloud services enable cloudbuild.googleapis.com
- gcloud services enable sourcerepo.googleapis.com

## Set Git config + Repo
- git config --global user.name "YOUR NAME"
- git config --global user.email "YOUR EMAIL"
- gcloud source repos create YOUR_REPO
- gcloud source repos clone YOUR_REPO
- cd YOUR_REPO
- echo readme > README.md
- git add .
- git commit -m "initial commit" README.md
- git push -u origin master

## Set TF State file
- PROJECT_ID=$(gcloud config get-value project)
- gsutil mb gs://${PROJECT_ID}-tfstate
- gsutil versioning set on gs://${PROJECT_ID}-tfstate

## Allow Cloud Build Service Account to manage resources in our project
- PROJECT_ID=$(gcloud config get-value project)
- CLOUDBUILD_SA="$(gcloud projects describe $PROJECT_ID --format 'value(projectNumber)')@cloudbuild.gserviceaccount.com"
- echo $CLOUDBUILD_SA
- gcloud projects add-iam-policy-binding $PROJECT_ID --member serviceAccount:$CLOUDBUILD_SA --role roles/editor	

# Inspired by
- https://cloud.google.com/architecture/managing-infrastructure-as-code