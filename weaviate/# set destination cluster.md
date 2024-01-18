# set destination cluster
gcloud config set project gcp-wow-wiq-success-ai-prod ; \
gcloud container clusters get-credentials weaviate-cluster-prod --region us-central1


# create work identity binding
gcloud iam service-accounts add-iam-policy-binding \
  --role roles/iam.workloadIdentityUser \
  --member "serviceAccount:gcp-wow-wiq-success-ai-prod.svc.id.goog[default/default]" \
  kubernetes-service-account@gcp-wow-wiq-success-ai-prod.iam.gserviceaccount.com

# create service account annotation
kubectl annotate serviceaccount \
  --namespace default \
   default \
   iam.gke.io/gcp-service-account=kubernetes-service-account@gcp-wow-wiq-success-ai-prod.iam.gserviceaccount.com