# Configure your environment and project
```
PROJECT_ID=$(gcloud config get-value project)
REGION=us-east1
echo "PROJECT_ID=${PROJECT_ID}"
echo "REGION=${REGION}"
```

```
gcloud services enable cloudbuild.googleapis.com cloudfunctions.googleapis.com run.googleapis.com logging.googleapis.com storage-component.googleapis.com aiplatform.googleapis.com
```

# Set up a Python virtual environment

## Create the venv
```
python3 -m venv gemini-streamlit
```

## Activate the venv
```
source gemini-streamlit/bin/activate
```

# Install application dependencies
```
pip install -r requirements.txt
```

# Deploy the app to Cloud Run

## Verify that the PROJECT_ID, and REGION environment variables are set:
```
echo "PROJECT_ID=${PROJECT_ID}"
echo "REGION=${REGION}"
```

## If these environment variables are not set, then run the command to set them:
```
PROJECT_ID=$(gcloud config get-value project)
REGION=us-east1
echo "PROJECT_ID=${PROJECT_ID}"
echo "REGION=${REGION}"
```

## Set environment variables for your service and artifact repository:
```
SERVICE_NAME='gemini-app-playground' # Name of your Cloud Run service.
AR_REPO='gemini-app-repo'            # Name of your repository in Artifact Registry that stores your application container image.
echo "SERVICE_NAME=${SERVICE_NAME}"
echo "AR_REPO=${AR_REPO}"
```

## Create the Docker repository
### To create the repository in Artifact Registry, run the command:
```
gcloud artifacts repositories create "$AR_REPO" --location="$REGION" --repository-format=Docker
```

### Set up authentication to the repository:
```
gcloud auth configure-docker "$REGION-docker.pkg.dev"
```

### To build the container image for your app, run the command:
```
gcloud builds submit --tag "$REGION-docker.pkg.dev/$PROJECT_ID/$AR_REPO/$SERVICE_NAME"
```

## Deploy and test your app on Cloud Run
```
gcloud run deploy "$SERVICE_NAME" \
  --port=8080 \
  --image="$REGION-docker.pkg.dev/$PROJECT_ID/$AR_REPO/$SERVICE_NAME" \
  --allow-unauthenticated \
  --region=$REGION \
  --platform=managed  \
  --project=$PROJECT_ID \
  --set-env-vars=PROJECT_ID=$PROJECT_ID,REGION=$REGION
```