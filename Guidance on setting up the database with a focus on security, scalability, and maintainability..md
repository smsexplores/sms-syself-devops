# Wanderlast Database Deployment

This document outlines the approach and steps for deploying a database for the Wanderlast application using Kubernetes and Helm. The deployment focuses on factors like persistence, scalability, security, and backup strategies to ensure a robust and reliable database infrastructure.

## Approach Overview

### 1. Requirement Analysis

- **Database Type:** 
  - Determine the appropriate database type (SQL vs. NoSQL) based on the application’s needs. For a simple backend application, a relational database like PostgreSQL would be suitable for structured data, while MongoDB could be used if the application requires flexibility in data schema.
  - The Wanderlast application needs to manage flexible or hierarchical data structures (like content that changes frequently or has varying fields), so MongoDB is more appropriate in this situation.
  
- **Persistence:**
  - The database needs to store data that persists across pod restarts, node failures, and other disruptions. In Kubernetes, this is achieved through Persistent Volumes (PV) and Persistent Volume Claims (PVC).

- **High Availability (HA):**
  - Consider deploying the database with high availability (replication, clustering) if necessary.
  - **When to Consider HA:**
    1. **Critical Application:** If the Wanderlast application is mission-critical, with users relying on it for essential services, HA is a must to avoid outages.
    2. **Data Integrity:** When the application handles sensitive or important data where loss or corruption would have severe consequences.
    3. **Scalability Needs:** HA can also support scalability, allowing the application to handle more users or transactions by distributing the load across multiple database instances.
  
- **Backup and Restore:** 
  - Plan for regular backups and the ability to restore the database in case of failure.

### 2. Architectural Design

- **Helm Chart for Database:**
  - **Database Selection:** Use an official Helm chart for MongoDB (e.g., Bitnami’s MongoDB Helm chart).
  - **Storage:** Define Persistent Volumes (PVs) and Persistent Volume Claims (PVCs) for the database to ensure data persists across pod restarts.
  - **Configuration:** Parameterize the database configurations (e.g., database name, user, password) in the `values.yaml` file.
  - **Networking:** Configure services and network policies to control access to the database, ensuring it’s only accessible by the application.

- **Resource Allocation:** Properly allocate CPU and memory resources based on the expected load.

## Tools and Technologies

### 1. Helm
- **Why:** Helm simplifies deploying complex applications, including databases, by offering pre-configured templates and customizable options. Using a well-maintained Helm chart ensures best practices and provides community support.

### 2. Persistent Volumes and Persistent Volume Claims (PV & PVC)
- **Why:** Databases need persistent storage to keep data safe even if a pod restarts or a node fails. PVs and PVCs in Kubernetes allow you to define and claim storage, ensuring that your data remains intact.

### 3. Kubernetes Secrets
- **Why:** Securely store sensitive information, like database credentials, within the cluster. This keeps unauthorized users from accessing critical data.

### 4. Backup Solutions (e.g., Velero)
- **Why:** Backup solutions are essential to recover database data in case of a failure or disaster. Tools like Velero offer backup, recovery, and migration capabilities for Kubernetes applications, ensuring that your data is always protected.

## Helm Chart Considerations

### 1. Chart Structure
- **Database Deployment:** Utilize the official Bitnami Helm chart for MongoDB, which comes with a reliable setup, including stateful sets, persistent volumes, and options for configuring replication.
- **Parameterization:** Customize the deployment by editing the `values.yaml` file. For example:
  - Set `mongodbUsername`, `mongodbPassword`, and `mongodbDatabase` for MongoDB.
- **Storage Class:** Specify the storage class in `values.yaml` to define the type of storage you need, such as `gp2` for AWS, ensuring the persistent volume is correctly provisioned.

### 2. Security
- **Secrets Management:** Store sensitive information like database credentials using Kubernetes Secrets. These secrets can be securely referenced in the Helm chart templates, keeping your data safe.
- **Network Policies:** Implement network policies to control and limit access to the database, making sure only the necessary pods can communicate with it. This adds an extra layer of security to your deployment.

### 3. High Availability (Optional)
- **Why:** If your application requires high availability, you can enable replication within the Helm chart configuration. This will deploy multiple replicas of the database, ensuring that data remains accessible even if one pod fails.

### 4. Backup and Restore
- **Why:** It’s crucial to have a backup and restore plan in place. Integrate a solution like Velero or use the database’s built-in tools to schedule regular backups. Ensure the backup location (such as an AWS S3 bucket) is properly defined and accessible, so you can recover your data whenever necessary.

## Deployment Process

1. Clone the repository and navigate to the Helm configs directory:
    ```bash
    git clone https://github.com/your-repo/wanderlast-helm-deployment.git
    cd wanderlast-helm-deployment/helm
    ```

2. Check the configs using dry-run:
    ```bash
    helm upgrade --install wanderlast . --dry-run --debug
    ```

3. Deploy the Helm chart:
    ```bash
    helm upgrade --install wanderlast .
    ```

4. Check the deployment:
    ```bash
    helm ls
    ```

5. Check the deployment status:
    ```bash
    kubectl get all
    ```

6. Get the URL for the service running in Minikube VM:
    ```bash
    minikube service wanderlast-helm-frontend --url
    ```

7. Test the app locally on Minikube VM:
    ```bash
    curl <URL>
    ```

8. Test the application outside the cluster via port-forwarding:
    ```bash
    kubectl get svc
    kubectl port-forward svc/wanderlast-helm-frontend <NodePort>:5173 --address 0.0.0.0 &
    ```

9. Test on browser:
    ```bash
    http://<PublicIP>:<NodePort>
    ```

10. Delete the deployment:
    ```bash
    helm uninstall wanderlast
    ```

## Backup and Monitoring

Set up monitoring for database performance and configure backup jobs to regularly back up the database.

## Conclusion

By using an official Helm chart for deploying the database, we ensure a standardized, well-supported, and easily customizable setup. The approach prioritizes data persistence, security, and maintainability, aligning with the broader goals of deploying a production-ready application in Kubernetes. This method also simplifies the integration of the database with the Wanderlast application, ensuring seamless communication and data handling.
