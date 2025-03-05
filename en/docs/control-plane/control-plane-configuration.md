# Configure Control Plane

## Configuring API Manager Control Plane with APK Gateway using APIM-APK Agent

### Prerequisites
- API Manager 4.5.0 (Please refer APIM <a href="https://apim.docs.wso2.com/en/latest/install-and-setup/install/installation-prerequisites/" target="_blank">installation guide</a> for more information)
- APK 1.3.0 (Please refer APK <a href="https://apk.docs.wso2.com/en/latest/install-and-setup/installation-prerequisites/" target="_blank">installation guide</a> for more information)
- APIM-APK Agent 1.3.0 (Please refer APIM-APK Agent <a href="https://apk.docs.wso2.com/en/latest/install-and-setup/install/installation-prerequisites/" target="_blank">installation guide</a> for more information)

#### Resources Requirements
- RAM: 10GB
- CPU: 8 Cores
- Storage: 10GB

### Configuring API Manager Control Plane

1.  Configure supported gateway types in API Manager.

    Add the following configuration to the deployment.toml file to provide supported gateway types to API Manager. Here, need to provide the supported gateway types as `APK`.

=== "Both"
    ``` toml
        [apim]
        gateway_type = "Regular,APK"
    ```
=== "APK Only"
    ``` toml
        [apim]
        gateway_type = "APK"
    ```

2. Change the default gateway environment if APK Only scenario.

    Add the following configuration to the deployment.toml file to provide APK gateway as the default gateway environment to API Manager. Here, need to provide the default gateway type as `APK` and other relevant configurations.

    ``` toml
    [[apim.gateway.environment]]
    name = "Default_production"
    type = "production"
    gateway_type = "APK"
    provider = "wso2"
    display_in_api_console = true
    description = "This is a hybrid gateway that handles both production and sandbox token traffic."
    show_as_token_endpoint_url = true
    service_url = "https://localhost:${mgt.transport.https.port}/services/"
    username= "${admin.username}"
    password= "${admin.password}"
    http_endpoint = "http://default.gw.wso2.com"
    https_endpoint = "https://default.gw.wso2.com"

    [[apim.gateway.environment]]
    name = "Default_sandbox"
    type = "sandbox"
    gateway_type = "APK"
    provider = "wso2"
    display_in_api_console = true
    description = "This is a hybrid gateway that handles both production and sandbox token traffic."
    show_as_token_endpoint_url = true
    service_url = "https://localhost:${mgt.transport.https.port}/services/"
    username= "${admin.username}"
    password= "${admin.password}"
    http_endpoint = "http://default.sandbox.gw.wso2.com"
    https_endpoint = "https://default.sandbox.gw.wso2.com"
    ```

### Configuring APK

1. Configure Control Plane related configuration in Common Controller

Add the following configuration to the common-log-conf file to provide control plane related configs to common-controller. Here, the host of the APIM-APK Agent should be provided.

``` yaml
    [commoncontroller.controlplane]
        enabled = true
        host = "apim-apk-agent-service.apk.svc.cluster.local"
        skipSSLVerification = true
```

2. Disable Hostname Verification for JWKS endpoint in APK Gateway (Optional)

Add the following configuration to disable SSL/hostname verification to JWKS validation call in enforcer in log-conf.yaml.
    
``` yaml
    [enforcer.client]
        skipSSL = false
        hostnameVerifier = "AllowAll"
```

### Configuring APIM-APK Agent

1. Configure Control Plane(APIM) related configuration in APIM-APK Agent

Add the following configuration to the values.yaml file to provide control plane related configs to APIM-APK Agent. Here, the host of the APIM-APK Agent should be provided.

``` yaml
    controlPlane:
      enabled: true
      serviceURL: https://apim-wso2am-cp-1-service.apk.svc.cluster.local:9443/
      username: admin
      password: admin
      environmentLabels: Default
      skipSSLVerification: true
      eventListeningEndpoints: amqp://admin:admin@apim-wso2am-cp-1-service.apk.svc.cluster.local:5672?retries='10'&connectdelay='30'
```

2. Configure Dataplane(APK Gateway) related configuration in APIM-APK Agent

Add the following configuration to the values.yaml file to provide dataplane related configs to APIM-APK Agent. Here, the host of the APK Gateway should be provided.

``` yaml
    dataPlane:
      enabled: true
      k8ResourceEndpoint: https://apk-wso2-apk-config-ds-service.apk.svc.cluster.local:9443/api/configurator/apis/generate-k8s-resources
      namespace: apk
```

## Next Steps

You can refer the [Control Plane Deployment](../control-plane/control-plane-deployment.md) regards to deploying the API Manager Control Plane with APK Gateway using APIM-APK Agent.