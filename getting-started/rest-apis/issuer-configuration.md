# Issuer Configuration

### Multi-tenancy

Since mid-Jan Q1/23 the Wallet Kit extended it's multi-issuer configurability to a complete multi-tenancy supported system.

All API endpoints for issuer configuration have the option for setting a _tenantId_.

The default _tenantId_ is "default".

### Creating a DID for a tenant&#x20;

POST /issuer-api/{tenantId}/config/createDid

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X 'POST' \
  'http://localhost:8080/issuer-api/default/config/createDid' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "method": "key",
}'
```


{% endtab %}

{% tab title="Body Schema" %}
POST this JSON body to the specified URL:

```json
{
    "method": "string*",
    "keyAlias": "string",
    "didWebDomain": "string",
    "didWebPath": "string"
}
```

Method being one of: \[ key, web, ebsi, iota, cheqd, jwk ]. Only the method is required. All "didWeb" prefixed options are only needed for creating a did:web.
{% endtab %}

{% tab title="Example Response" %}
Response: `did:key:z6MknaJ7YLdkCq1QV2tcwVSY5uVfUKGfBdigo7g4PyipTnCc`


{% endtab %}
{% endtabs %}

### Retrieve and update tenant configuration

GET /issuer-api/{tenantId}/config/getConfiguration

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X 'GET' \
  'http://localhost:8080/issuer-api/default/config/getConfiguration' \
  -H 'accept: application/json'
```
{% endtab %}

{% tab title="Example Response" %}
```json
{
    "issuerUiUrl": "https://issuer.cheqd.walt-test.cloud",
    "issuerApiUrl": "https://issuer.cheqd.walt-test.cloud/issuer-api/default",
    "issuerClientName": "walt.id Issuer Portal",
    "wallets": {
        "walt.id": {
            "id": "walt.id",
            "url": "https://wallet.cheqd.walt-test.cloud",
            "description": "walt.id web wallet",
            "presentPath": "api/siop/initiatePresentation/",
            "receivePath" : "api/siop/initiateIssuance/"
        }
    }
}
```
{% endtab %}
{% endtabs %}

### **Update issuer configuration to include our generated DID**&#x20;

POST /issuer-api/{tenantId}/config/setConfiguration

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X 'POST' \
  'https://wallet.cheqd.walt-test.cloud/issuer-api/default/config/setConfiguration' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d 'vv JSON BELOW vv'
```
{% endtab %}

{% tab title="Example Response" %}
```json
{
    "issuerUiUrl": "https://issuer.cheqd.walt-test.cloud",
    "issuerApiUrl": "https://issuer.cheqd.walt-test.cloud/issuer-api/default",
    "issuerClientName": "walt.id Issuer Portal",
    "issuerDid": "did:key:z6MknaJ7YLdkCq1QV2tcwVSY5uVfUKGfBdigo7g4PyipTnCc",
    "wallets": {
        "walt.id": {
            "id": "walt.id",
            "url": "https://wallet.cheqd.walt-test.cloud",
            "description": "walt.id web wallet",
            "presentPath": "api/siop/initiatePresentation/",
            "receivePath" : "api/siop/initiateIssuance/"
        }
    }
}
```

The configuration was amended to include the "issuerDid" attribute with the DID from "Creating a DID for a tenant".


{% endtab %}
{% endtabs %}
