# GenaiStudyJam

## Do Task 1 manually

### Run the below commands in cloud shell
## until task 3
```
export ZONE=$(gcloud compute instances list --filter="name=(generative-ai-jupyterlab)" --format="value(zone)")
export REGION=${ZONE::-2}
git clone https://github.com/GoogleCloudPlatform/generative-ai.git
cd generative-ai/gemini/sample-apps/gemini-streamlit-cloudrun
rm -f Dockerfile
curl -o Dockerfile https://raw.githubusercontent.com/sujanMidatani7/GenaiStudyJam/main/Dockerfile
curl -o chef.py https://raw.githubusercontent.com/sujanMidatani7/GenaiStudyJam/main/chef.py
gcloud storage cp chef.py gs://$DEVSHELL_PROJECT_ID-generative-ai/
python3 -m venv gemini-streamlit
source gemini-streamlit/bin/activate
pip install -r requirements.txt
GCP_PROJECT=$DEVSHELL_PROJECT_ID
GCP_REGION=$REGION
streamlit run chef.py \
--browser.serverAddress=localhost \
--server.enableCORS=false \
--server.enableXsrfProtection=false \
--server.port 8080

```
# click on the link and generate an recipe

## for next steps run below commands

```
AR_REPO='chef-repo'
SERVICE_NAME='chef-streamlit-app'
PROJECT=[your project id]
REGION=[your region]
gcloud artifacts repositories create "$AR_REPO" --location="$REGION" --repository-format=Docker
gcloud builds submit --tag "$REGION-docker.pkg.dev/$PROJECT/$AR_REPO/$SERVICE_NAME"

```
next ->
```
gcloud run deploy "$SERVICE_NAME" \
  --port=8080 \
  --image="$REGION-docker.pkg.dev/$PROJECT/$AR_REPO/$SERVICE_NAME" \
  --allow-unauthenticated \
  --region=$REGION \
  --platform=managed  \
  --project=$PROJECT \
  --set-env-vars=PROJECT=$PROJECT,REGION=$REGION
```
