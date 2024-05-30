# F5 App Stack

1. [Create an App Stack Site](https://docs.cloud.f5.com/docs/how-to/site-management/create-voltstack-site)
2. Retrieve the global kubeconfig

```shell
export DISTRIBUTED_CLOUD_TENANT=mytenantname
# find tenant id in the F5 Distributed Cloud GUI at
# Account -> Account Settings -> Tenant Overview -> Tenant ID
export DISTRIBUTED_CLOUD_TENANT_ID=mytenantnamewithextensionfoundintheconsole
# create an API token in the F5 Distributed Cloud GUI at
# Account -> Account Settings -> Credentials -> Add Credentials 
# set Credential Type to API Token, not API Certificate
export DISTRIBUTED_CLOUD_API_TOKEN=myapitoken
export DISTRIBUTED_CLOUD_SITE_NAME=appstacksitename
export DISTRIBUTED_CLOUD_NAMESPACE=mydistributedcloudnamespace
export DISTRIBUTED_CLOUD_APP_STACK_NAMESPACE=portkeyai
export DISTRIBUTED_CLOUD_APP_STACK_SITE=myappstacksite
export DISTRIBUTED_CLOUD_SERVICE_NAME=portkeyai
# adjust the expiry date to a time no more than 90 days in the future
export KUBECONFIG_CERT_EXPIRE_DATE="2021-09-14T09:02:25.547659194Z"
export PORTKEY_GATEWAY_FQDN=the.host.nameof.theservice
export PORTKEY_PROVIDER=openai
export PORTKEY_PROVIDER_AUTH_TOKEN=authorizationtoken

curl --location --request POST 'https://$DISTRIBUTED_CLOUD_TENANT.console.ves.volterra.io/api/web/namespaces/system/sites/$DISTRIBUTED_CLOUD_SITE_NAME/global-kubeconfigs' \
--header 'Authorization: APIToken $DISTRIBUTED_CLOUD_API_TOKEN' \
--header 'Access-Control-Allow-Origin: *' \
--header 'x-volterra-apigw-tenant: $DISTRIBUTED_CLOUD_TENANT'\
--data-raw '{"expirationTimestamp":"$KUBECONFIG_CERT_EXPIRE_DATE"}'
```

Save the response in a YAML file for later use.\
[more detailed instructions for retrieving the App Stack kubeconfig file](https://f5cloud.zendesk.com/hc/en-us/articles/4407917988503-How-to-download-kubeconfig-via-API-or-vesctl)

3. Copy the deployment YAML

```shell
wget https://raw.githubusercontent.com/Portkey-AI/gateway/main/deployment.yaml
```

4. Apply the manifest

```shell
export KUBECONFIG=path/to/downloaded/global/kubeconfig/in/step/two
# apply the file downloaded in step 3
kubectl apply -f deployment.yaml
```

5. Create Origin Pool

```shell
# create origin pool
curl --request POST \
  --url https://$DISTRIBUTED_CLOUD_TENANT.console.ves.volterra.io/api/config/namespaces/$DISTRIBUTED_CLOUD_NAMESPACE/origin_pools \
  --header 'authorization: APIToken $DISTRIBUTED_CLOUD_API_TOKEN' \
  --header 'content-type: application/json' \
  --data '{"metadata": {"name": "$DISTRIBUTED_CLOUD_SERVICE_NAME","namespace": "$DISTRIBUTED_CLOUD_NAMESPACE","labels": {},"annotations": {},"description": "","disable": false},"spec": {"origin_servers": [{"k8s_service": {"service_name": "$DISTRIBUTED_CLOUD_SERVICE_NAME.$DISTRIBUTED_CLOUD_APP_STACK_NAMESPACE","site_locator": {"site": {"tenant": "$DISTRIBUTED_CLOUD_TENANT_ID","namespace": "system","name": "$DISTRIBUTED_CLOUD_APP_STACK_SITE"}},"inside_network": {}},"labels": {}}],"no_tls": {},"port": 8787,"same_as_endpoint_port": {},"healthcheck": [],"loadbalancer_algorithm": "LB_OVERRIDE","endpoint_selection": "LOCAL_PREFERRED","advanced_options": null}}'
```

or [use the UI](https://docs.cloud.f5.com/docs/how-to/app-networking/origin-pools) 5. Create an HTTP Load Balancer, including header injection of Portkey provider and credentials

```shell
curl --request POST \
  --url https://$DISTRIBUTED_CLOUD_TENANT.console.ves.volterra.io/api/config/namespaces/$DISTRIBUTED_CLOUD_NAMESPACE/http_loadbalancers \
  --header 'authorization: APIToken $DISTRIBUTED_CLOUD_API_TOKEN' \
  --header 'content-type: application/json' \
  --data '{"metadata": {"name": "$DISTRIBUTED_CLOUD_SERVICE_NAME","namespace": "$DISTRIBUTED_CLOUD_NAMESPACE","labels": {},"annotations": {},"description": "","disable": false},"spec": {"domains": ["$PORTKEY_GATEWAY_FQDN"],"https_auto_cert": {"http_redirect": true,"add_hsts": false,"tls_config": {"default_security": {}},"no_mtls": {},"default_header": {},"enable_path_normalize": {},"port": 443,"non_default_loadbalancer": {},"header_transformation_type": {"default_header_transformation": {}},"connection_idle_timeout": 120000,"http_protocol_options": {"http_protocol_enable_v1_v2": {}}},"advertise_on_public_default_vip": {},"default_route_pools": [{"pool": {"tenant": "$DISTRIBUTED_CLOUD_TENANT_ID","namespace": "$DISTRIBUTED_CLOUD_NAMESPACE","name": "$DISTRIBUTED_CLOUD_SERVICE_NAME"},"weight": 1,"priority": 1,"endpoint_subsets": {}}],"origin_server_subset_rule_list": null,"routes": [],"cors_policy": null,"disable_waf": {},"add_location": true,"no_challenge": {},"more_option": {"request_headers_to_add": [{"name": "x-portkey-provider","value": "$PORTKEY_PROVIDER","append": false},{"name": "Authorization","value": "Bearer $PORTKEY_PROVIDER_AUTH_TOKEN","append": false}],"request_headers_to_remove": [],"response_headers_to_add": [],"response_headers_to_remove": [],"max_request_header_size": 60,"buffer_policy": null,"compression_params": null,"custom_errors": {},"javascript_info": null,"jwt": [],"idle_timeout": 30000,"disable_default_error_pages": false,"cookies_to_modify": []},"user_id_client_ip": {},"disable_rate_limit": {},"malicious_user_mitigation": null,"waf_exclusion_rules": [],"data_guard_rules": [],"blocked_clients": [],"trusted_clients": [],"api_protection_rules": null,"ddos_mitigation_rules": [],"service_policies_from_namespace": {},"round_robin": {},"disable_trust_client_ip_headers": {},"disable_ddos_detection": {},"disable_malicious_user_detection": {},"disable_api_discovery": {},"disable_bot_defense": {},"disable_api_definition": {},"disable_ip_reputation": {},"disable_client_side_defense": {},"csrf_policy": null,"graphql_rules": [],"protected_cookies": [],"host_name": "","dns_info": [],"internet_vip_info": [],"system_default_timeouts": {},"jwt_validation": null,"disable_threat_intelligence": {},"l7_ddos_action_default": {},}}'
```

or [use the UI](https://docs.cloud.f5.com/docs/how-to/app-networking/http-load-balancer) 6. Test the service

```shell
curl --request POST \
  --url https://$PORTKEY_GATEWAY_FQDN/v1/chat/completions \
  --header 'content-type: application/json' \
  --data '{"messages": [{"role": "user","content": "Say this might be a test."}],"max_tokens": 20,"model": "gpt-4"}'
```

in addition to the response headers, you should get a response body like

```json
{
  "id": "chatcmpl-abcde......09876",
  "object": "chat.completion",
  "created": "0123456789",
  "model": "gpt-4-0321",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "This might be a test."
      },
      "logprobs": null,
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 14,
    "completion_tokens": 6,
    "total_tokens": 20
  },
  "system_fingerprint": null
}
```
