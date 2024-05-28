# AWS

This enterprise-focused document provides comprehensive instructions for deploying the Portkey software on AWS, tailored to meet the needs of large-scale, mission-critical applications. It includes specific recommendations for component sizing, high availability, disaster recovery, and integration with monitoring systems.

## Components and Sizing Recommendations

| Component                               | Options                                                                   | Sizing Recommendations                                                                                                                                 |
| --------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| AI Gateway                              | Deploy as a Docker container in your Kubernetes cluster using Helm Charts | <p>AWS EC2 t4g.medium instance, with at least 4GiB of memory and two vCPUs<br><br>For high reliability, deploy across multiple Availability Zones.</p> |
| Logs Store (optional)                   | Hosted MongoDB, Document DB or AWS S3                                     | Each log document is \~10kb in size (uncompressed)                                                                                                     |
| Cache (Prompts, Configs & Virtual Keys) | Elasticache or self-hosted Redis                                          | Deploy in the same VPC as the Portkey Gateway.                                                                                                         |

## Deployment Steps

#### Prerequisites

Ensure the following tools are installed:

* Docker
* Kubectl&#x20;
* Helm (v3 or above)

### Step 1: Clone the Portkey repo containing helm chart

```
git clone https://github.com/Portkey-AI/helm-chart
```

### Step 2: Update values.yaml for Helm

Modify the values.yaml file in the Helm chart directory to include the Docker registry credentials and necessary environment variables. You can find the sample file at `./helm-chart/helm/enterprise/values.yaml`

**Image Credentials Configuration**

```
imageCredentials:
  name: portkey-enterprise-registry-credentials
  create: true
  registry: kubernetes.io/dockerconfigjson
  username: <docker-username>
  password: <docker-password>
```

The Portkey team will share the credentials for your image

**Environment Variables Configuration**

```yaml
environment:
  data:
    SERVICE_NAME: "gateway_enterprise"
    PORTKEY_CLIENT_AUTH: "<client key shared by Portkey>"
    PORTKEY_ORGANISATION_ID: "<your_organisation_id>"
    ORGANISATIONS_TO_SYNC="<comma separated if multiple>"
    LOG_STORE: "<your_log_store>"
    # If you're using MongoDB as Logs Store
    MONGO_DB_CONNECTION_URL: "<your_mongo_db_url>"
    MONGO_DATABASE: "<your_mongo_db>"
    MONGO_COLLECTION_NAME: "<your_mongo_collection>"
    # If you're using S3-compatible store as Logs Store
    S3_REGION: "<your_s3_region>"
    S3_ACCESS_KEY: "<your_s3_access_key>"
    S3_SECRET_KEY: "<your_s3_secret_key>"
    S3_GENERATIONS_BUCKET: "<your_s3_bucket>"
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

**Notes on the Log Store** `LOG_STORE` can be

* an S3 compatible store (AWS S3 `s3`, GCS `gcs`, Wasabi `wasabi`)
* or a MongoDB store (Hosted MongoDB `mongo`, AWS DocumentDB `mongo`)

If the `LOG_STORE` is `mongo`, the following environment variables are needed

```
MONGO_DB_CONNECTION_URL: 
MONGO_DATABASE:
MONGO_COLLECTION_NAME:
```

If the `LOG_STORE` is `s3` or `wasabi` or `gcs`, the following values are mandatory

```
S3_REGION:
S3_ACCESS_KEY:
S3_SECRET_KEY:
S3_GENERATIONS_BUCKET:
```

All the above mentioned are S3 Compatible document storages and interoperable with S3 API. You need to generate the `Access Key` and `Secret Key` from the respective providers.

**Notes on Cache** If `CACHE_STORE` is set as `redis`, a redis instance will also get deployed in the cluster. If you are using custom redis, then leave it blank.

The following values are mandatory

```
REDIS_URL: 
REDIS_TLS_ENABLED: 
```

`REDIS_URL` defaults to `redis://redis:6379` and `REDIS_TLS_ENABLED` defaults to `false`.

**Notes on Analytics Store** This is hosted in Portkey’s control plane and these credentials will be shared by the Portkey team.

### Step 3: Deploy using Helm Charts

Navigate to the directory containing your Helm chart and run the following command to deploy the application:

```
helm install portkey-gateway ./helm/enterprise --namespace portkeyai --create-namespace
```

This command installs the Helm chart into the `portkeyai` namespace.

### Step 4: Verify the Deployment

Check the status of your deployment to ensure everything is running correctly:

```
kubectl get pods -n portkeyai
```

### Step 5: Port Forwarding (Optional)

To access the service over internet, use port forwarding:

```
kubectl port-forward <pod-name> -n portkeyai 443:8787
```

Replace `<pod-name>` with the name of your pod.

### Uninstalling the Deployment

If you need to remove the deployment, run:

```
helm uninstall portkey-app --namespace portkeyai
```

This command will uninstall the Helm release and clean up the resources.

## Network Configuration

### Step 1: Allow access to the service

To make the service accessible from outside the cluster, define a Service of type LoadBalancer in your values.yaml or Helm templates. Specify the desired port for external access.

```
service:
  type: LoadBalancer
  port: <desired_port>
  targetPort: 8787
```

Replace `<desired_port>` with the port number for external access with the port the application listens on internally.

### Step 2: Ensure Outbound Network Access

By default, Kubernetes allows full outbound access, but if your cluster has NetworkPolicies that restrict egress, configure them to allow outbound traffic.

Example NetworkPolicy for Outbound Access:

```
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

This allows the gateway to access LLMs hosted within your VPC and outside as well. This also enables connection for the sync service to the Portkey Control Plane.

### Step 3: Configure Inbound Access for Portkey Control Plane

Ensure the Portkey control plane can access the service either over the internet or through VPC peering.

**Over the Internet:**

* Ensure the LoadBalancer security group allows inbound traffic on the specified port.
* Document the public IP/hostname and port for the control plane connection.

**Through VPC Peering:**

Set up VPC peering between your AWS account and the control plane's AWS account. Requires manual setup by Portkey Team.
