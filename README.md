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
Service Port Verification
To ensure your Grafana and Prometheus services are correctly configured, follow these steps to verify the service ports.

Check Grafana Service Ports
Run the following command to check the configuration of the Grafana service:

sh
kubectl get svc grafana-ext -o yaml
In the output, look for the ports section. It should resemble:

yaml
ports:
- port: 80
  targetPort: 3000
  nodePort: 30895
Check Prometheus Service Ports
Run the following command to check the configuration of the Prometheus service:

sh
kubectl get svc prometheus-server-ext-new -o yaml
Similarly, check the ports section:

yaml
ports:
- port: 80
  targetPort: 9090
  nodePort: 31517
Port-Forward Commands
Based on the output from the above commands, adjust your port-forwarding commands accordingly.

For Grafana
If the nodePort is 30895 and the targetPort is 3000, but you encounter port issues, adjust the local port in the port-forward command:

sh

kubectl port-forward svc/grafana-ext 8080:80 --address 0.0.0.0 &
Access Grafana at http://localhost:8080.

For Prometheus
If the nodePort is 31517 and the targetPort is 9090, use the following command:

sh

kubectl port-forward svc/prometheus-server-ext-new 8081:80 --address 0.0.0.0 &
Access Prometheus at http://localhost:8081.

