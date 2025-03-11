You can deploy the API directly into Kubernetes Gateway using API Schema definition and Kubernetes Gateway configuration file using the command below.

```
curl --location 'https://api.am.wso2.com:9095/api/deployer/1.3.0/apis/deploy' \
--header 'Content-Type: multipart/form-data' \
--header 'Accept: application/yaml' \
--header 'Authorization: Bearer <Access Token From IDP>' \
--form 'apkConfiguration=@"/Users/user/EmployeeService.apk-conf"' \
--form 'definitionFile=@"/Users/user/EmployeeServiceDefinition.json"'
```

Once we invoke the above API, the response body would look like below.

```yaml
id: "048f821480bb19de6b64973a4b69109af286cfef"
name: "EmployeeServiceAPI"
basePath: "/test"
version: "3.14"
type: "REST"
organization: "default"
defaultVersion: false
endpointConfigurations:
    production:
        - endpoint: "http://employee-service:8080"
operations:
- target: "/employee"
    verb: "GET"
    secured: true
    scopes: []
- target: "/employee"
    verb: "POST"
    secured: true
    scopes: []
```