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