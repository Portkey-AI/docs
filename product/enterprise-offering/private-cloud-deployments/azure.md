# Azure

{% hint style="warning" %}
These documents are in beta and are WIP. Please reach out in case you face issues with these instructions.
{% endhint %}

This enterprise-focused document provides comprehensive instructions for deploying the Portkey software on Microsoft Azure, tailored to meet the needs of large-scale, mission-critical applications. It includes specific recommendations for component sizing, high availability, disaster recovery, and integration with monitoring systems.

## Components and Sizing Recommendations

### Component: AI Gateway

* **Deployment:** Deploy as a Docker container in your Kubernetes cluster using Helm Charts.
* **Instance Type:** Azure Standard B2ms instance, with at least 4GiB of memory and two vCPUs.
* **High Availability:** Deploy across multiple zones for high reliability.

### Component: Logs Store (optional)

* **Options:** Azure Cosmos DB, Azure Blob Storage.
* **Sizing:** Each log document is \~10kb in size (uncompressed).

### Component: Cache (Prompts, Configs & Virtual Keys)

* **Options:** Azure Cache for Redis or self-hosted Redis.
* **Deployment:** Deploy in the same VNet as the Portkey Gateway.

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

Can be fetched from a secrets store

```yaml
environment:
  data:
    SERVICE_NAME: "gateway_enterprise"
    PORTKEY_CLIENT_AUTH: "<client key shared by Portkey>"
    PORTKEY_ORGANISATION_ID: "<your_organisation_id>"
    ORGANISATIONS_TO_SYNC="<comma separated if multiple>"
    LOG_STORE: "<your_log_store>"
    # If you're using Cosmos DB as Logs Store
    COSMOS_DB_CONNECTION_URL: "<your_cosmos_db_url>"
    COSMOS_DATABASE: "<your_cosmos_db>"
    COSMOS_COLLECTION_NAME: "<your_cosmos_collection>"
    # If you're using Azure Blob Storage as Logs Store
    AZURE_STORAGE_ACCOUNT_NAME: "<your_storage_account_name>"
    AZURE_STORAGE_ACCOUNT_KEY: "<your_storage_account_key>"
    AZURE_CONTAINER_NAME: "<your_container_name>"
    # Analytics Store credentials shared by Portkey
    ANALYTICS_STORE: "<your_analytics_store>"
    ANALYTICS_STORE_USER: "<your_clickhouse_user>"
    ANALYTICS_STORE_PASSWORD: "<your_clickhouse_password>"
    ANALYTICS_STORE_HOST: "<your_clickhouse_host>"
    ANALYTICS_STORE_TABLE: "<your_clickhouse_log_table>"
    ANALYTICS_STORE_FEEDBACK_TABLE: "<your_clickhouse_feedback_table>"
    # Your cache details
    CACHE_STORE: "<your_cache_store>"
    REDIS_URL: "<your_redis_url>"
    REDIS_TLS_ENABLED: "<true or false>"
    # For semantic cache, when using Pinecone
    PINECONE_NAMESPACE="<your_pinecone_namespace>"
    PINECONE_INDEX_HOST="<pinecode_index_host>"
    PINECONE_API_KEY="<pinecone>"       
    SEMCACHE_OPENAI_EMBEDDINGS_API_KEY="<open_ai_key_for_embeddings>"
    SEMCACHE_OPENAI_EMBEDDINGS_MODEL= "text-embedding-3-small"
```

**Notes on the Log Store**

`LOG_STORE` can be:

* `s3`, for Azure Blob Storage
* `mongo`, for Azure Cosmos DB

If the `LOG_STORE` is `mongo`, the following environment variables are needed:

```yaml
MONGO_DB_CONNECTION_URL: 
MONGO_DATABASE:
MONGO_COLLECTION_NAME:
```

If the `LOG_STORE` is `s3`, the following values are mandatory:

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

_This allows the gateway to access LLMs hosted within your VNet and outside as well. This also enables connection for the sync service to the Portkey Control Plane._

### Step 3: Configure Inbound Access for Portkey Control Plane

Ensure the Portkey control plane can access the service either over the internet or through VNet peering.

**Over the Internet:**

* Ensure the LoadBalancer security group allows inbound traffic on the specified port.
* Document the public IP/hostname and port for the control plane connection.

**Through VNet Peering:**

* Set up VNet peering between your Azure account and the control plane's Azure account. Requires manual setup by Portkey Team.

This guide provides the necessary steps and configurations to deploy Portkey on Azure effectively, ensuring high availability, scalability, and integration with your existing infrastructure.
