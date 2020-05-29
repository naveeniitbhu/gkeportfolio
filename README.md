
export PROJECT_ID=project-portfolio-278518  //Check Project ID
docker build -t gcr.io/${PROJECT_ID}/portfolio-app:v1 .
gcloud auth configure-docker
docker push gcr.io/${PROJECT_ID}/portfolio-app:v1 //update vesion during next deploy

gcloud config set compute/zone asia-east1-a
gcloud container clusters create portfolio-app-ctr --num-nodes=2

# connect to cluster via command line
gcloud container clusters get-credentials portfolio-app-ctr --zone asia-east1-a --project project-portfolio-278518
	
gcloud compute instances list

# Installing Helm //todo update Helm to verdion 3.x.x
curl -LO https://git.io/get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
 
kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
helm init --service-account tiller --upgrade

helm search nginx-ingress
helm install stable/nginx-ingress --name my-nginx --set rbac.create=true

helm search cert-manager
kubectl create namespace cert-manager
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install --name cert-manager --namespace cert-manager --version v0.15.0 jetstack/cert-manager --set installCRDs=true


### config DNS and domain
gcloud auth list
gcloud config set account ngnaven@gmail.com
gcloud auth login ngnaven@gmail.com


