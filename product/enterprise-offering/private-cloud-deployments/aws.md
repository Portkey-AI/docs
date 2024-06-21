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
LOG_STORE_REGION: 
LOG_STORE_ACCESS_KEY: 
LOG_STORE_SECRET_KEY: 
LOG_STORE_GENERATIONS_BUCKET:
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



The following are mandatory and are shared by the Portkey Team.

```
PORTKEY_CLIENT_AUTH:
ORGANISATIONS_TO_SYNC:
```

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

## Required Permissions

To ensure the smooth operation of Portkey AI in your private cloud deployment on AWS, specific permissions are required based on the type of log store you are using. Below are the details for S3 or MongoDB compliant databases.

**S3 Bucket**

If using S3 as the log store, the following IAM policy permissions are required:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::YOUR_BUCKET_NAME",
                "arn:aws:s3:::YOUR_BUCKET_NAME/*"
            ]
        }
    ]
}
```

Please replace `YOUR_BUCKET_NAME` with your actual bucket name.

**MongoDB Compliant Database**

If using a MongoDB compliant database, ensure the AI Gateway has access to the database. The database user should have following role:

```json
{
    "roles": [
        {
            "role": "readWrite",
            "db": "YOUR_DATABASE_NAME"
        }
    ]
}
```

The `readWrite` role provides the necessary read and write access to the specified database. Please replace `YOUR_DATABASE_NAME` with your actual database name.

**Cache Store - Redis**

The Portkey Gateway image ships with a redis installed. You can choose to use the inbuilt redis or connect to an outside Redis instance.

1. **Redis as Part of the Image:**\
   No additional permissions or networking configurations are required.
2.  **Separate Redis Instance:**

    The gateway requires permissions to perform read and write operations on the Redis instance. The redis connections can be configured with or without TLS.
