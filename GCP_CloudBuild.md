# GCP Cloud Build

## Conditional Build

Example:

```yaml
steps:
  # Grant the Object Admin role to the buckets
  - name: google/cloud-sdk
    args: ['gsutil', 'iam', 'ch', 'serviceAccount:${_SERVICE_ACCOUNT}:objectAdmin', 'gs://${_UNSCANNED_BUCKET_PREFIX}-${_LOB1}']
  - name: google/cloud-sdk
    args: ['gsutil', 'iam', 'ch', 'serviceAccount:${_SERVICE_ACCOUNT}:objectAdmin', 'gs://${_UNSCANNED_BUCKET_PREFIX}-${_LOB2}']
  - name: google/cloud-sdk
    args: ['gsutil', 'iam', 'ch', 'serviceAccount:${_SERVICE_ACCOUNT}:objectAdmin', 'gs://${_UNSCANNED_BUCKET_PREFIX}-${_LOB3}']
  - name: google/cloud-sdk
    args: ['gsutil', 'iam', 'ch', 'serviceAccount:${_SERVICE_ACCOUNT}:objectAdmin', 'gs://${_UNSCANNED_BUCKET_PREFIX}-${_LOB4}']
  - name: google/cloud-sdk
    args: ['gsutil', 'iam', 'ch', 'serviceAccount:${_SERVICE_ACCOUNT}:objectAdmin', 'gs://${_CLEAN_BUCKET_PREFIX}-${_LOB1}']
  - name: google/cloud-sdk
    args: ['gsutil', 'iam', 'ch', 'serviceAccount:${_SERVICE_ACCOUNT}:objectAdmin', 'gs://${_CLEAN_BUCKET_PREFIX}-${_LOB2}']
  - name: google/cloud-sdk
    args: ['gsutil', 'iam', 'ch', 'serviceAccount:${_SERVICE_ACCOUNT}:objectAdmin', 'gs://${_CLEAN_BUCKET_PREFIX}-${_LOB3}']
  - name: google/cloud-sdk
    args: ['gsutil', 'iam', 'ch', 'serviceAccount:${_SERVICE_ACCOUNT}:objectAdmin', 'gs://${_CLEAN_BUCKET_PREFIX}-${_LOB4}']
  - name: google/cloud-sdk
    args: ['gsutil', 'iam', 'ch', 'serviceAccount:${_SERVICE_ACCOUNT}:objectAdmin', 'gs://${_QUARANTINED_BUCKET}']

  # Docker Build
  - name: 'gcr.io/cloud-builders/docker'
    args: ['build',
           '--file', '/workspace/app-scanner-dockerfile',
           '-t', 'us-central1-docker.pkg.dev/${PROJECT_ID}/${_ARTIFACT_REGISTRY_REPO}/app:${SHORT_SHA}',
           '.']

  # Create Google Artifact Registry Repository conditionally
  - name: gcr.io/google.com/cloudsdktool/cloud-sdk
    entrypoint: bash
    args:
      - -c
      - |
        if [ $(gcloud artifacts repositories list | grep ${_ARTIFACT_REGISTRY_REPO} | wc -l) -gt 0 ]
        then
          echo "The Artifact Registry Repository ${_ARTIFACT_REGISTRY_REPO} has already been created."
        else
          echo "The Artifact Registry Repository ${_ARTIFACT_REGISTRY_REPO} needs to be created."
          gcloud artifacts repositories create ${_ARTIFACT_REGISTRY_REPO} --location=${_REGION} --repository-format=docker
        fi

  # Docker push to Google Artifact Registry Repository
  - name: 'gcr.io/cloud-builders/docker'
    args: ['push',  'us-central1-docker.pkg.dev/${PROJECT_ID}/${_ARTIFACT_REGISTRY_REPO}/app:${SHORT_SHA}']

  # Deploy to Cloud Run service
  - name: google/cloud-sdk
    args: ['gcloud', 'beta', 'run', 'deploy', '${_SERVICE_NAME}',
           '--image=us-central1-docker.pkg.dev/${PROJECT_ID}/${_ARTIFACT_REGISTRY_REPO}/app:${SHORT_SHA}',
           '--region', '${_REGION}', '--platform', 'managed',
          '--cpu', '4', '--memory', '8Gi', '--min-instances', '4', '--no-cpu-throttling',
          '--service-account=${_SERVICE_ACCOUNT}',
          '--set-env-vars', 'CONFIG_FILE=./config_${PROJECT_ID}.json',
          '--no-allow-unauthenticated']

  # Allow the malware-scanner service account to invoke the Cloud Run service
  - name: google/cloud-sdk
    args: ['gcloud', 'run', 'services', 'add-iam-policy-binding', '${_SERVICE_NAME}', '--region', '${_REGION}', '--member', 'serviceAccount:${_SERVICE_ACCOUNT}', '--role', 'roles/run.invoker']

  # Create 4 eventarcs conditionally
  - name: gcr.io/google.com/cloudsdktool/cloud-sdk
    entrypoint: bash
    args:
      - -c
      - |
        if [ $(gcloud eventarc triggers list | grep trigger-${_SERVICE_NAME}-${_LOB1} | wc -l) -gt 0 ]
        then
          echo "The eventarc trigger trigger-${_SERVICE_NAME}-${_LOB1} has already been created."
        else
          echo "The eventarc trigger trigger-${_SERVICE_NAME}-${_LOB1} needs to be created."
          gcloud eventarc triggers create trigger-${_SERVICE_NAME}-${_LOB1} --destination-run-service=${_SERVICE_NAME} --destination-run-region=${_REGION} --location=${_LOCATION} --event-filters type=google.cloud.storage.object.v1.finalized --event-filters bucket=${_UNSCANNED_BUCKET_PREFIX}-${_LOB1} --service-account=${_SERVICE_ACCOUNT}
        fi
        if [ $(gcloud eventarc triggers list | grep trigger-${_SERVICE_NAME}-${_LOB2} | wc -l) -gt 0 ]
        then
          echo "The eventarc trigger trigger-${_SERVICE_NAME}-${_LOB2} has already been created."
        else
          echo "The eventarc trigger trigger-${_SERVICE_NAME}-${_LOB2} needs to be created."
          gcloud eventarc triggers create trigger-${_SERVICE_NAME}-${_LOB2} --destination-run-service=${_SERVICE_NAME} --destination-run-region=${_REGION} --location=${_LOCATION} --event-filters type=google.cloud.storage.object.v1.finalized --event-filters bucket=${_UNSCANNED_BUCKET_PREFIX}-${_LOB2} --service-account=${_SERVICE_ACCOUNT}
        fi
        if [ $(gcloud eventarc triggers list | grep trigger-${_SERVICE_NAME}-${_LOB3} | wc -l) -gt 0 ]
        then
          echo "The eventarc trigger trigger-${_SERVICE_NAME}-${_LOB3} has already been created."
        else
          echo "The eventarc trigger trigger-${_SERVICE_NAME}-${_LOB3} needs to be created."
          gcloud eventarc triggers create trigger-${_SERVICE_NAME}-${_LOB3} --destination-run-service=${_SERVICE_NAME} --destination-run-region=${_REGION} --location=${_LOCATION} --event-filters type=google.cloud.storage.object.v1.finalized --event-filters bucket=${_UNSCANNED_BUCKET_PREFIX}-${_LOB3} --service-account=${_SERVICE_ACCOUNT}
        fi
        if [ $(gcloud eventarc triggers list | grep trigger-${_SERVICE_NAME}-${_LOB4} | wc -l) -gt 0 ]
        then
          echo "The eventarc trigger trigger-${_SERVICE_NAME}-${_LOB4} has already been created."
        else
          echo "The eventarc trigger trigger-${_SERVICE_NAME}-${_LOB4} needs to be created."
          gcloud eventarc triggers create trigger-${_SERVICE_NAME}-${_LOB4} --destination-run-service=${_SERVICE_NAME} --destination-run-region=${_REGION} --location=${_LOCATION} --event-filters type=google.cloud.storage.object.v1.finalized --event-filters bucket=${_UNSCANNED_BUCKET_PREFIX}-${_LOB4} --service-account=${_SERVICE_ACCOUNT}
        fi

# Store images in Google Artifact Registry Repository
images:
  - us-central1-docker.pkg.dev/${PROJECT_ID}/${_ARTIFACT_REGISTRY_REPO}/app:${SHORT_SHA}

substitutions:
  _REGION: us-central1
  _LOCATION: us
  _ARTIFACT_REGISTRY_REPO: cloudrun-deploy
  _UNSCANNED_BUCKET_PREFIX: unscanned-${PROJECT_ID}-app
  _CLEAN_BUCKET_PREFIX: clean-${PROJECT_ID}-app
  _QUARANTINED_BUCKET: quarantine-${PROJECT_ID}-app
  _SERVICE_ACCOUNT: sa-malware-scanner@${PROJECT_ID}.iam.gserviceaccount.com
  _SERVICE_NAME: app-malware-scanner
  _LOB1: lob1
  _LOB2: lob2
  _LOB3: lob3
  _LOB4: lob4
```
