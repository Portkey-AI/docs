# Architecture

## Overview

Portkey for enterprises is preferably deployed in a hybrid cloud architecture with the AI gateway hosted in the Customer VPC which manages all the traffic from the Customer applications to the respective LLMs. An attached cache manages the keys, access control, routing rules, guardrails and all other traffic management.

The Control Plane hosted in Portkey's VPC provides the Control Panel UI that helps you control the Data Plane including users, prompts, virtual keys, access management, configs while providing key observability information like logs and analytics.

The architecture comprises:

* **Data Plane** (In Customer VPC): Hosts the AI gateway with an attached cache and optionally a log store.
* **Control Plane** (In Portkey VPC): Hosts the Control Panel UI, the metrics store, a transactional DB and optionally the log store.



All the metrics, logs, prompts and other data stored in the Control Plane is encrypted and stored in an isolated data shard. The Customer can optionally provide their own encryption keys from a KMS. Portkey connect to AWS KMS and provides envelope encryption across the Control Plane.

## Role of the Control Plane

The control plane takes away the hassle of hosting and managing the moving parts of the AI gateway while maintaining the highest levels of availability and flexibility. The Portkey Control Plane takes care of the

* Prompt Management, Versioning and Deployments
* Aggregating metrics effectively across the gateway traffic
* UI for viewing logs and analytics with advanced filters, metadata and feedback
* Organisation and team management including Roles & Permissions
* Config management and versioning for the Gateway
* and more

without customers needing to manage all these systems in-house. The privately deployed AI gateway can seamlessly sync and decrypt information from the Control Plane so all the data & traffic stays within the Customer's cloud.

## Data flow between Customer Cloud and Portkey Cloud

1. **AI gateway will send anonymised request metrics to the analytics store**. This contains information such as model used, prompt IDs, configs used, tokens, cost, request timings that are then made available on Portkey's analytics dashboards. Sample data that is sent.\

2.  **AI Gateway maintains a sync with the Control Plane** through a heartbeat to update the cache stores every 30 seconds. Data for prompt updates, config updates, virtual keys, and API keys are fetched, decrypted and stored in the gateway's cache.\


    During any LLM call, the AI Gateway directly utilizes data stored in cache.\

3. **If Logs are stored in Customer Cloud**: Portkey's UI will make a call to the AI gateway to fetch a log by id when a customer views it on the dashboard. **If Logs are stored in Portkey Cloud**: The AI gateway will encrypt and send the log to be stored in the Portkey Logs store. In this case, no inbound connection is needed from Portkey to Customer Cloud. \[Sample Log Data]

## New features and patch management

Portkey is able to launch newer features to the Customer's deployment automatically, for most features. For anything that involves changes on the AI Gateway, new releases and patches will be made available through the container registry.