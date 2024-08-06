## PROD Deployment via custom Helm Charts
1. Install minikube cluster on a t2.large instance and navigate to the directoy having helm configs
```bash
cd helm
```
2. Before deployment check the configs using dry-run 
```bash
helm upgrade --install wanderlast . --dry-run --debug
```
3. Deploy the helm chart
```bash
helm upgrade --install wanderlast .
```
4. Check the deployment
```bash
helm ls
```
5. Check the deployment status
```bash
kubectl get all
```
6. Get the URL for the service running in minikube VM
```bash
minikube service wanderlast-helm-frontend --url
```
7. Test the app locally on minikube VM
```bash
curl <URL>
```
8. Now to test the application outside the cluster via port-forwarding
```bash
kubectl get svc
kubectl port-forward svc/wanderlast-helm-frontend NodePort-port:5173 --address 0.0.0.0 &
```
9. Now to test on browser 
```bash
http://PublicIP:NodePort
```
10. Delete the deployment
```bash
helm uninstall wanderlast .
```
