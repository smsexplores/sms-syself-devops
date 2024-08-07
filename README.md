# Wanderlast Application Deployment using Helm

This repository contains Helm charts for deploying the Wanderlast sample backend application to a Kubernetes cluster. It also includes setup instructions for Minikube and observability tools like Prometheus and Grafana.

## Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Helm](https://helm.sh/docs/intro/install/)
- An EC2 instance (t2.large recommended)
- [MobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html)

## Setting Up EC2 Instance

1. **Launch an EC2 instance** (t2.large) on your preferred cloud provider and connect to it using MobaXterm.

2. **Connect to your EC2 instance**:
   - Open MobaXterm.
   - Start a new session (SSH).
   - Enter your EC2 instance's public IP and use your private key for authentication.

3. **Install Minikube** on the EC2 instance:
   ```sh
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   sudo install minikube-linux-amd64 /usr/local/bin/minikube
4. **Install Docker**
   sudo apt-get update
   sudo apt-get install -y docker.io
   sudo usermod -aG docker $USER
   newgrp docker
5.**Start Minikube**
   minikube start --driver=docker --memory=8192 --cpus=4
# Deploying the Application
### Clone the repository and navigate to the Helm configs directory:
    git clone https://github.com/your-repo/wanderlast-helm-deployment.git
    cd wanderlast-helm-deployment/helm
### Check the configs using dry-run:
    helm upgrade --install wanderlast . --dry-run --debug
### Deploy the Helm chart:
    helm upgrade --install wanderlast .
### Check the deployment:
    helm ls
### Check the deployment status:
    kubectl get all
### Get the URL for the service running in Minikube VM:
    minikube service wanderlast-helm-frontend --url
### Test the app locally on Minikube VM:
    curl <URL>
### Test the application outside the cluster via port-forwarding:
    kubectl get svc
    kubectl port-forward svc/wanderlast-helm-frontend <NodePort>:5173 --address 0.0.0.0 &
###  Test on browser:
    php
    http://<PublicIP>:<NodePort>
### Delete the deployment:
    helm uninstall wanderlast
## Observability Setup and Monitoring
### Prometheus
#### Add Prometheus community maintained Helm chart:
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo update
#### Test Prometheus using dry-run and validate the chart configs:
    helm install prometheus prometheus-community/prometheus --dry-run --debug
#### Deploy Prometheus after validation:
    helm install prometheus prometheus-community/prometheus
#### Expose Prometheus service to access Prometheus-server using the browser:
    kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext
#### Test the service for Prometheus-server-ext:
    kubectl get svc
    kubectl port-forward svc/prometheus-server-ext <NodePort>:80 --address 0.0.0.0 &
#### Navigate to browser:
      http://<PublicIP>:<NodePort>
### Grafana
#### Add the Grafana community maintained Helm chart:
    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update
#### Test Grafana using dry-run and validate the chart configs:
      helm install grafana grafana/grafana --dry-run --debug
#### Deploy Grafana after validation:
      helm install grafana grafana/grafana
#### Expose Grafana service to access Grafana using the browser:
      kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext
#### Test the service for grafana-ext:
      kubectl get svc
      kubectl port-forward svc/grafana-ext <NodePort>:80 --address 0.0.0.0 &
#### Navigate to browser:
      http://<PublicIP>:<NodePort>
#### Get the 'admin' user password:
      kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
### Setup datasource in Grafana dashboard:

        Go to Home --> Add Datasource --> Prometheus
         In Datasources, use the connection string for our Prometheus-server:
              kubectl get svc prometheus-server
#### Example Connection URL:
        http://<prometheus-server-cluster-IP>:<Port>
### Create Dashboard to visualize the charts:
        Go to dashboards and import dashboard ID 3662.
