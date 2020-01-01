# Helm-with-GKE
This repository describes the steps to deploy kubernetes deployment files using helm on GKE Cluster

## Connect to GKE Cluster
` gcloud container clusters get-credentials cluster-name --region region-name` 

## Install Helm
` curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh`

## Install Helm Client
` chmod 700 get_helm.sh`
`./get_helm.sh`

## Initiate Helm
` helm init`

## Create Helm Chart
` helm create kube-deployment`<br />
A folder with kube-deployment will be created with files mentioned in kube-deployment folder present on this repository
Modify the charts as per your requirements.

## Check if chart is well-formed
` helm lint ./kube-deployment`

## Create Service Account for tiller 
` kubectl create serviceaccount --namespace kube-system tiller`

## Create Cluster Role Binding for tiller to give permissions to helm client for making deployments
` kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller`

## Adding the ServiceAccount created to the helm client
` kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}' `
## Install Helm chart
` helm install --name helm-deployment ./kube-deployment`
" --name" is used for naming the helm release


## Interpret the charts before actual installation
` helm install -n helm-deployment ./kube-deployment --dry-run --debug`

## Listing the helm releases
` helm ls --all`

## Upgrade Helm charts
` helm upgrade helm-deployment ./kube-deployment`

## Delete Helm Release
` helm delete --purge helm-deployment`

