# GCP

This enterprise-focused document provides comprehensive instructions for deploying the Portkey software on Google Cloud Platform (GCP), tailored to meet the needs of large-scale, mission-critical applications. It includes specific recommendations for component sizing, high availability, disaster recovery, and integration with monitoring systems.

## Components and Sizing Recommendations

### Component: AI Gateway

* **Deployment:** Deploy as a Docker container in your Kubernetes cluster using Helm Charts.
* **Instance Type:** GCP n1-standard-2 instance, with at least 4GiB of memory and two vCPUs.
* **High Availability:** Deploy across multiple zones for high reliability.

### Component: Logs Store (optional)

* **Options:** Hosted MongoDB, Google Cloud Storage (GCS), or Google Firestore.
* **Sizing:** Each log document is \~10kb in size (uncompressed).

### Component: Cache (Prompts, Configs & Virtual Keys)

* **Options:** Google Memorystore for Redis or self-hosted Redis.
* **Deployment:** Deploy in the same VPC as the Portkey Gateway.

## Deployment Steps

### Prerequisites

Ensure the following tools are installed:

* Docker
* Kubectl
* Helm (v3 or above)

### Step 1: Clone the Portkey Repo Containing Helm Chart

```bash
git clone https://github.com/Portkey-AI/helm-chart
```

### Step 2: Update values.yaml for Helm

Modify the `values.yaml` file in the Helm chart directory to include the Docker registry credentials and necessary environment variables. You can find the sample file at `./helm-chart/helm/enterprise/values.yaml`.

**Image Credentials Configuration**

```yaml
imageCredentials:
  name: portkey-enterprise-registry-credentials
  create: true
  registry: kubernetes.io/dockerconfigjson
  username: <docker-username>
  password: <docker-password>
```

_The Portkey team will share the credentials for your image._

**Environment Variables Configuration**

These can be stored & fetched from a vault as well

```yaml
environment:
  ...
  data:
    SERVICE_NAME: 
    LOG_STORE: 
    MONGO_DB_CONNECTION_URL: 
    MONGO_DATABASE: 
    MONGO_COLLECTION_NAME: 
    LOG_STORE_REGION: 
    LOG_STORE_ACCESS_KEY: 
    LOG_STORE_SECRET_KEY: 
    LOG_STORE_GENERATIONS_BUCKET: 
    ANALYTICS_STORE: 
    ANALYTICS_STORE_ENDPOINT: 
    ANALYTICS_STORE_USER: 
    ANALYTICS_STORE_PASSWORD: 
    ANALYTICS_LOG_TABLE: 
    ANALYTICS_FEEDBACK_TABLE: 
    CACHE_STORE: 
    REDIS_URL: 
    REDIS_TLS_ENABLED: 
    PORTKEY_CLIENT_AUTH: 
    ORGANISATIONS_TO_SYNC: 
```

**Notes on the Log Store**

`LOG_STORE` can be:

* Google Cloud Storage (`gcs`)
* Hosted MongoDB (`mongo`)

If the `LOG_STORE` is `mongo`, the following environment variables are needed:

```yaml
MONGO_DB_CONNECTION_URL: 
MONGO_DATABASE:
MONGO_COLLECTION_NAME:
```

If the `LOG_STORE` is `gcs`, the following values are mandatory:

```yaml
LOG_STORE_REGION: 
LOG_STORE_ACCESS_KEY: 
LOG_STORE_SECRET_KEY: 
LOG_STORE_GENERATIONS_BUCKET: 
```

_You need to generate the Access Key and Secret Key from the respective providers._

**Notes on Cache**

If `CACHE_STORE` is set as `redis`, a Redis instance will also get deployed in the cluster. If you are using custom Redis, then leave it blank. The following values are mandatory:

```yaml
REDIS_URL: 
REDIS_TLS_ENABLED: 
```

`REDIS_URL` defaults to `redis://redis:6379` and `REDIS_TLS_ENABLED` defaults to `false`.

**Notes on Analytics Store**

This is hosted in Portkey’s control plane and these credentials will be shared by the Portkey team.

The following are mandatory and are shared by the Portkey Team.

```
PORTKEY_CLIENT_AUTH:
ORGANISATIONS_TO_SYNC:
```

### Step 3: Deploy Using Helm Charts

Navigate to the directory containing your Helm chart and run the following command to deploy the application:

```bash
helm install portkey-gateway ./helm/enterprise --namespace portkeyai --create-namespace
```

_This command installs the Helm chart into the `portkeyai` namespace._

### Step 4: Verify the Deployment

Check the status of your deployment to ensure everything is running correctly:

```bash
kubectl get pods -n portkeyai
```

### Step 5: Port Forwarding (Optional)

To access the service over the internet, use port forwarding:

```bash
kubectl port-forward <pod-name> -n portkeyai 443:8787
```

_Replace `<pod-name>` with the name of your pod._

### Uninstalling the Deployment

If you need to remove the deployment, run:

```bash
helm uninstall portkey-app --namespace portkeyai
```

_This command will uninstall the Helm release and clean up the resources._

## Network Configuration

### Step 1: Allow Access to the Service

To make the service accessible from outside the cluster, define a Service of type `LoadBalancer` in your `values.yaml` or Helm templates. Specify the desired port for external access.

```yaml
service:
  type: LoadBalancer
  port: <desired_port>
  targetPort: 8787
```

_Replace `<desired_port>` with the port number for external access with the port the application listens on internally._

### Step 2: Ensure Outbound Network Access

By default, Kubernetes allows full outbound access, but if your cluster has NetworkPolicies that restrict egress, configure them to allow outbound traffic.

**Example NetworkPolicy for Outbound Access:**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-all-egress
  namespace: portkeyai
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 0.0.0.0/0
```

_This allows the gateway to access LLMs hosted within your VPC and outside as well. This also enables connection for the sync service to the Portkey Control Plane._

### Step 3: Configure Inbound Access for Portkey Control Plane

Ensure the Portkey control plane can access the service either over the internet or through VPC peering.

**Over the Internet:**

* Ensure the LoadBalancer security group allows inbound traffic on the specified port.
* Document the public IP/hostname and port for the control plane connection.

**Through VPC Peering:**

* Set up VPC peering between your GCP account and the control plane's GCP account. Requires manual setup by Portkey Team.

This guide provides the necessary steps and configurations to deploy Portkey on GCP effectively, ensuring high availability, scalability, and integration with your existing infrastructure.
