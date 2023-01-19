# Credential Issuance API

Issuance is very simple, in fact, you only need to call a single HTTP POST endpoint:

### Issue Credential

POST /issuer-api/{tenantId}/credentials/issuance/request

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X 'POST' \
  'http://0.0.0.0:8080/issuer-api/default/credentials/issuance/request?isPreAuthorized=true&walletId=$walletId' \
  -H 'accept: text/plain' \
  -H 'Content-Type: application/json' \
  -d 'SEE EXAMPLE BODY'
```

\
**Wallet**\
When issuing a credential, you will have to pre-select what type of wallet shall be used.

There are three types of wallets:

1. Mobile wallets:
   * Cross device flow: User uses another device (desktop pc, laptop) than where their wallet is installed (smartphone).
   * In this case, you will use the virtual wallet "x-device", which creates an "openid-initiate-issuance://..." URL.
2. Web wallets:
   * Same device flow: User uses the same device that their wallet is installed on.
   * In this case, you will have to additionally ask what web wallet the user is using (if you aren't forcing them to use your or an specific web wallet).
   * e.g. "walt.id", which creates an "[https://wallet.walt.id/](https://wallet.walt.id/)..." URL.
3. App wallets
   * Theoretically same device flow, but also uses the virtual wallet "x-device" for creating an "openid-initiate-issuance://..." URL.

In the default configuration you can use the wallet "walt.id" ([https://wallet.walt.id](https://wallet.walt.id/)), or the virtual wallet "x-device" for the cross device flow ("openid-initiate-issuance://...").

The selected walletId is the id configured in the mappings seen in wallet\_kit-configuration.m
{% endtab %}

{% tab title="Example Body Schema" %}
```json
{
    "credentials": [
        {
        "credentialData": {
            "my data": "..."
        },
        "type": "string"
        }
    ]
}
```
{% endtab %}

{% tab title="Example Body" %}
```json
{
    "credentials": [
        {
        "credentialData": {
            "credentialSubject": {
                "currentAddress":["1 Boulevard de la Liberté, 59800 Lille"],"dateOfBirth":"1993-04-08","familyName":"DOE","firstName":"Jane","gender":"FEMALE","id":"did:ebsi:2AEMAqXWKYMu1JHPAgGcga4dxu7ThgfgN95VyJBJGZbSJUtp","nameAndFamilyNameAtBirth":"Jane DOE","personalIdentifier":"0904008084H","placeOfBirth":"LILLE, FRANCE"}
        },
        "type": "VerifiableId"
        }
    ]
}
```
{% endtab %}

{% tab title="Example Response" %}
Answers with a redirect (header "Location") to the issuance URL, e.g. [`http://localhost:3000/api/siop/initiateIssuance/?issuer=http%3A%2F%2Flocalhost%3A8080%2Fissuer-api%2Fdefault%2Foidc%2F&credential_type=VerifiableId&pre-authorized_code=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxYTYyMzJiMy1hZDE0LTQ3YTYtYjMxNi00NzUzZjJhM2Q2NWYiLCJwcmUtYXV0aG9yaXplZCI6dHJ1ZX0.TGYpvRwE9Qc7-BSMLAA-u3Kweyf8ks2w3ulBpSyZQiI&user_pin_required=false`](http://localhost:3000/api/siop/initiateIssuance/?issuer=http%3A%2F%2Flocalhost%3A8080%2Fissuer-api%2Fdefault%2Foidc%2F\&credential\_type=VerifiableId\&pre-authorized\_code=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxYTYyMzJiMy1hZDE0LTQ3YTYtYjMxNi00NzUzZjJhM2Q2NWYiLCJwcmUtYXV0aG9yaXplZCI6dHJ1ZX0.TGYpvRwE9Qc7-BSMLAA-u3Kweyf8ks2w3ulBpSyZQiI\&user\_pin\_required=false)``
{% endtab %}
{% endtabs %}
