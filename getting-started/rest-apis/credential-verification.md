# Credential Verification

There are 3 ways to integrate the verification flows into your application:

1) Same-device (web-based) flow
2) Cross-device flow (with QR code)
3) Via webhooks (callback)

In all cases, first make sure you have your tenant configured with all the verification policies, etc. that fit your requirements.

Then start the presentation flow, using the methods described below.

## Same device flow

`GET /verifier-api/<tenantId>/present?walletId=walt.id&vcType=VerifiableId`

**Supported parameters:**

| Key | Description | Example |
|---    |---        |---      |
| tenantId   | Path parameter, the tenant ID for this request       | /verifier-api/**default**/present?...        |
| walletId         | Wallet ID as configured in the tenant configuration | walletId=walt.id |
| vcType           | Credential type that should be requested in the presentation request. **Can be specified multiple times** | vcType=VerifiableId&vcType=Europass
| pdByReference      | If true, the presentation definition will be passed as a URI in the presentation request (see [`presentation_definition_uri`](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html#name-presentation_definition_uri)), otherwise a full PD object is included in the [`presentation_definition`](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html#name-presentation_definition-par) parameter. | pdByReference=false
| verifierUiUrl     | Web redirection URL, on which the verification result will be received. The browser will be navigated to this URL with the sub-path **/success**, containing an access_token parameter, which can be used to obtain the verification result, **overrides** the [`verifierUiUrl`](../../configuration-and-setup/verifier-portal-setup.md#external-urls) configuration parameter in the tenant configuration | verifierUiUrl=https://myservice.example.com/verification/callback/, **will be called like:** https://myservice.example.com/verification/callback/success/?access_token=abc123
| verificationCallbackUrl | Webhook to actively callback with the verification result (see also [Verification callbacks](#verification-callbacks)), the URL needs to be configured as allowed webhook in the tenant configuration. |

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X 'GET' \
  'http://localhost:8080/verifier-api/default/present?walletId=walt.id&vcType=VerifiableId&verificationCallbackUrl=http://wallet.local:5555/callback/5dd9f971-9cd2-4c41-a40f-5340ac6d1fd3' \
  -H 'accept: application/json'
```
{% endtab %}

{% tab title="Response Example" %}
Answers with a redirect (header "Location") to the verification URL (at your specified wallet). e.g.

```html
http://localhost:3000/api/siop/initiatePresentation/?scope=openid&presentation_definition=%7B%22format%22+%3A+null%2C+%22id%22+%3A+%221%22%2C+%22input_descriptors%22+%3A+%5B%7B%22constraints%22+%3A+%7B%22fields%22+%3A+%5B%7B%22filter%22+%3A+%7B%22const%22%3A+%22VerifiableId%22%7D%2C+%22id%22+%3A+null%2C+%22path%22+%3A+%5B%22%24.type%22%5D%2C+%22purpose%22+%3A+null%7D%5D%7D%2C+%22format%22+%3A+null%2C+%22group%22+%3A+null%2C+%22id%22+%3A+%221%22%2C+%22name%22+%3A+null%2C+%22purpose%22+%3A+null%2C+%22schema%22+%3A+null%7D%5D%2C+%22name%22+%3A+null%2C+%22purpose%22+%3A+null%2C+%22submission_requirements%22+%3A+null%7D&response_type=vp_token&redirect_uri=http%3A%2F%2Flocalhost%3A8080%2Fverifier-api%2Fdefault%2Fverify&state=1c4ef304-5a8f-4074-9b90-a4f2b26ce0b4&nonce=1c4ef304-5a8f-4074-9b90-a4f2b26ce0b4&client_id=http%3A%2F%2Flocalhost%3A8080%2Fverifier-api%2Fdefault%2Fverify&response_mode=form_post
```

(a local wallet at localhost was configured in this example case)

By telling the wallet what vcType is required, it will usually filter the list of credentials in the user UI to only display relevant credentials of the specified type.
{% endtab %}
{% endtabs %}

In the same-device flow the browser gets navigated to this API URL, with the given parameters

The walletkit redirects to the wallet and on completion, the walletkit redirects the browser back to the application. 

The web redirection URL is configured in the tenant configuration parameter [`verifierUiUrl`](../../configuration-and-setup/verifier-portal-setup.md#external-urls), or can be overridden using the `verifierUiUrl` URL parameter.

The `verifierUiUrl` is called with the sub-path /success and an `access_token` parameter, which can be used to [retrieve the verification result data](#verification-result).

**Example flow**

Navigate the browser to:

`https://wallet.walt-test.cloud/verifier-api/default/present?walletId=walt.id&vcType=VerifiableId&vcType=Europass&verifierUiUrl=https://myservice.example.com/callback/`

The browser will be redirected to the wallet. The user will be presented the presentation request and asked to confirm it.

Eventually the browser will be redirected back to the application like this:

`https://myservice.example.com/callback/success/?access_token=abc123`

**Note** the subpath `/success/` being added to the `verifierUiUrl`!

Use the access token to retrieve the verification result from the walletkit like described in [Verification result](#verification-result).

## Cross device flow

`GET /verifier-api/{tenantId}/presentXDevice`

**Supported parameters:**

| Key | Description | Example |
|---    |---        |---      |
| tenantId   | Path parameter, the tenant ID for this request       | /verifier-api/**default**/presentXDevice?...        |
| vcType           | Credential type that should be requested in the presentation request. **Can be specified multiple times** | vcType=VerifiableId&vcType=Europass
| pdByReference      | If true, the presentation definition will be passed as a URI in the presentation request (see [`presentation_definition_uri`](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html#name-presentation_definition_uri)), otherwise a full PD object is included in the [`presentation_definition`](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html#name-presentation_definition-par) parameter. | pdByReference=false

{% tabs %}
{% tab title="CURL" %}

```bash
curl -X 'GET' \
  'http://localhost:8080/verifier-api/default/presentXDevice?vcType=VerifiableId' \
  -H 'accept: application/json'
```
{% endtab %}

{% tab title="Example Response" %}
```json
{
  "requestId": "82dcbb5a-e5b3-447d-bc25-b22d7cfa4445",
  "url": "openid://?scope=openid&presentation_definition=%7B%22format%22+%3A+null%2C+%22id%22+%3A+%221%22%2C+%22input_descriptors%22+%3A+%5B%7B%22constraints%22+%3A+%7B%22fields%22+%3A+%5B%7B%22filter%22+%3A+%7B%22const%22%3A+%22VerifiableId%22%7D%2C+%22id%22+%3A+null%2C+%22path%22+%3A+%5B%22%24.type%22%5D%2C+%22purpose%22+%3A+null%7D%5D%7D%2C+%22format%22+%3A+null%2C+%22group%22+%3A+null%2C+%22id%22+%3A+%221%22%2C+%22name%22+%3A+null%2C+%22purpose%22+%3A+null%2C+%22schema%22+%3A+null%7D%5D%2C+%22name%22+%3A+null%2C+%22purpose%22+%3A+null%2C+%22submission_requirements%22+%3A+null%7D&response_type=vp_token&redirect_uri=http%3A%2F%2Flocalhost%3A8080%2Fverifier-api%2Fdefault%2Fverify&state=82dcbb5a-e5b3-447d-bc25-b22d7cfa4445&nonce=82dcbb5a-e5b3-447d-bc25-b22d7cfa4445&client_id=http%3A%2F%2Flocalhost%3A8080%2Fverifier-api%2Fdefault%2Fverify&response_mode=post"
}
```
{% endtab %}
{% endtabs %}

For starting a cross-device presentation flow, call this API with the required parameters to create a presentation request.

As a response, you get an object containing a `requestId` and a `url`.

Your application should now render the `url` from the response object as a QR code, or deep-link.

The `requestId` from the response object can be used to query the verification request status, using the API endpoint described [here](#cross-device-verification-request-status).

The wallet can scan the rendered QR code and parse the presentation request. Eventually it will post the presentation response to the walletkit backend, which will update the [request status](#cross-device-verification-request-status).

When the request is fulfilled, the response of the [verification request status](#cross-device-verification-request-status) API, will contain a URI with the `access_token` parameter, which can be used to fetch the presentation result using the [Verification result](#verification-result) API.

### Cross-device verification request status

`GET /verifier-api/{tenantId}/verify/isVerified`

**Supported parameters:**

| Key | Description | Example |
|---    |---        |---      |
| tenantId   | Path parameter, the tenant ID for this request       | /verifier-api/**default**/verify/isVerified?...        |
| state | The **request ID** as returned by the `presentXDevice` API call | state=req123

{% tabs %}
{% tab title="CURL" %}

```bash
curl -X 'GET' \
  'http://localhost:8080/verifier-api/default/verify/isVerified?state=req123' \
  -H 'accept: application/json'
```
{% endtab %}

{% tab title="Example Response" %}
```
https://myservice.example.com/callback/success/?access_token=abc123
```
{% endtab %}
{% endtabs %}

The application calls this API, after starting a [cross-device presentation flow](#cross-device-flow), using the request ID received in the response object for the `state` parameter of this API.

This API will return a status `404 Not found`, while it is waiting for the cross-device request to be fulfilled.

When the request is fulfilled, it will return a status `200 OK` with the redirection URL in the response body.

From the redirection URL, the application can parse the `access_token` parameter, to fetch the [verification result](#verification-result).

## Verification result

`GET /verifier-api/<tenantID>/auth?access_token=abc123`

**Parameters**

| Key | Description | Example |
|---    |---        |---      |
| tenantId   | Path parameter, the tenant ID for this request       | /verifier-api/**default**/auth?...        |
| access_token | The access token, as returned in web redirection URL or cross-device verification response | access_token=abc123 |

The response of this request will be a verification result, like described in the section [Example verification result](#example-json-body-youll-receive-with-a-webhook-callback-for-the-verifiableid-demo) section.

## Verification callbacks

As seen in the first example, you can optionally set a callback:

`https://wallet.cheqd.walt-test.cloud/verifier-api/default/present?walletId=walt.id&vcType=VerifiableId&verificationCallbackUrl=http://wallet.local:5555/callback/5dd9f971-9cd2-4c41-a40f-5340ac6d1fd3` (Note `&verificationCallbackUrl=`)

When setting a callback, the Wallet Kit will then call this callback as webhook when the user shares a credential (in the walt.id demo wallet: actually clicks "Share credentials").

Before answering the user, the Wallet Kit will send out an HTTP Post request using the parameters described below.

* When the callback answers HTTP statuses
  * Statuses:
    * OK (200),
    * NoContent (201),
    * or Accepted (202)
  * the Wallet Kit will mark the callback as accepted and continue its normal operation.

***

* When the callback answers HTTP statuses
  * Statuses:
    * MovedPermanently (301),
    * Found (302),
    * TemporaryRedirect (307),
    * or PermanentRedirect (308)
  * the Wallet Kit will abort its operation and redirect the user (using the `Location` header) to the `Location` specified by the callback.
  * This is useful for redirecting to pages _based_ on what credentials the user shares or change depending on the data of the credentials.

***

* When the callback answers HTTP statuses
  * Statuses:
    * Unauthorized (401),
    * or Forbidden (403)
  * the Wallet Kit will invalidate the status and set overallValid to false, even if the selected `VerificationPolicy`s would result in an `overallValid = true`.



### Callback data to work with

When you set up your webhook to listen for such a Wallet Kit callback, you have a variety of data to work with:

* state: (UUID) ID of the verification session
* subject: (DID) DID of the user that created the shared presentation
* auth\_token: (JWT\[Subject]) Signed representation of subject = DID
* vps: (List\[VPVerificationResult])
  * VPVerificationResult:
    * vp: (JWT) Signed representation of the Verifiable Presentation
      * Including DIDs, issuance dates (attribute `iat`, `nbf`), etc...
      * and obviously the actual Verifiable Credential (in attribute `vc`)
    * vcs: (List\[JWT])
      * Signed Verifiable Credentials sourced from the Verifiable Presentation (VC in attribute `vc`, issuance date `iat` `nbf`, Subject DID `sub`, ...)
    * verification\_result: (VerificationResult)
      * valid: Boolean
        * overall valid state
      * policyResults: Map\[Policy -> Boolean]
        * e.g. `policyResults={SignaturePolicy=true, ChallengePolicy=true, PresentationDefinitionPolicy=true}`

#### Example JSON body you'll receive with a webhook callback for the VerifiableID demo

```json
{
    "isValid": true,
    "state": "41676715-a069-4f43-baee-57bdbeaf2c98",
    "subject": "did:key:z6Mkidk54UoPVhPqkWExngRj3Cd33jAdNcKfd6ePkrCZEbjU",
    "vps": [
        {
            "vcs": [
                {
                    "context": [
                        {
                            "_isObject": false,
                            "properties": {},
                            "uri": "https://www.w3.org/2018/credentials/v1"
                        }
                    ],
                    "credentialSchema": {
                        "id": "https://api.preprod.ebsi.eu/trusted-schemas-registry/v1/schemas/0xb77f8516a965631b4f197ad54c65a9e2f9936ebfb76bae4906d33744dbcc60ba",
                        "properties": {
                            "id": "https://api.preprod.ebsi.eu/trusted-schemas-registry/v1/schemas/0xb77f8516a965631b4f197ad54c65a9e2f9936ebfb76bae4906d33744dbcc60ba"
                        },
                        "type": "FullJsonSchemaValidator2021"
                    },
                    "credentialSubject": {
                        "id": "did:key:z6Mkidk54UoPVhPqkWExngRj3Cd33jAdNcKfd6ePkrCZEbjU",
                        "properties": {
                            "currentAddress": [
                                "1 Boulevard de la Liberté, 59800 Lille"
                            ],
                            "dateOfBirth": "1993-04-08",
                            "familyName": "DOE",
                            "firstName": "Jane",
                            "gender": "FEMALE",
                            "nameAndFamilyNameAtBirth": "Jane DOE",
                            "personalIdentifier": "0904008084H",
                            "placeOfBirth": "LILLE, FRANCE"
                        }
                    },
                    "id": "urn:uuid:7cf18a60-e624-44b0-8ea8-aec630a2fb66",
                    "issuanceDate": "2023-01-09T15:41:50Z",
                    "issued": "2023-01-09T15:41:50Z",
                    "issuer": {
                        "_isObject": false,
                        "id": "did:key:z6Mkvrfpsv1pB3qwEXJAWtz6u7zgib2PZYj5zRg8gJfCsJqv",
                        "properties": {}
                    },
                    "issuerId": "did:key:z6Mkvrfpsv1pB3qwEXJAWtz6u7zgib2PZYj5zRg8gJfCsJqv",
                    "jwt": "eyJraWQiOiJkaWQ6a2V5Ono2TWt2cmZwc3YxcEIzcXdFWEpBV3R6NnU3emdpYjJQWllqNXpSZzhnSmZDc0pxdiN6Nk1rdnJmcHN2MXBCM3F3RVhKQVd0ejZ1N3pnaWIyUFpZajV6Umc4Z0pmQ3NKcXYiLCJ0eXAiOiJKV1QiLCJhbGciOiJFZERTQSJ9.eyJzdWIiOiJkaWQ6a2V5Ono2TWtpZGs1NFVvUFZoUHFrV0V4bmdSajNDZDMzakFkTmNLZmQ2ZVBrckNaRWJqVSIsIm5iZiI6MTY3MzI3ODkxMCwiaXNzIjoiZGlkOmtleTp6Nk1rdnJmcHN2MXBCM3F3RVhKQVd0ejZ1N3pnaWIyUFpZajV6Umc4Z0pmQ3NKcXYiLCJpYXQiOjE2NzMyNzg5MTAsInZjIjp7InR5cGUiOlsiVmVyaWZpYWJsZUNyZWRlbnRpYWwiLCJWZXJpZmlhYmxlQXR0ZXN0YXRpb24iLCJWZXJpZmlhYmxlSWQiXSwiQGNvbnRleHQiOlsiaHR0cHM6Ly93d3cudzMub3JnLzIwMTgvY3JlZGVudGlhbHMvdjEiXSwiaWQiOiJ1cm46dXVpZDo3Y2YxOGE2MC1lNjI0LTQ0YjAtOGVhOC1hZWM2MzBhMmZiNjYiLCJpc3N1ZXIiOiJkaWQ6a2V5Ono2TWt2cmZwc3YxcEIzcXdFWEpBV3R6NnU3emdpYjJQWllqNXpSZzhnSmZDc0pxdiIsImlzc3VhbmNlRGF0ZSI6IjIwMjMtMDEtMDlUMTU6NDE6NTBaIiwiaXNzdWVkIjoiMjAyMy0wMS0wOVQxNTo0MTo1MFoiLCJ2YWxpZEZyb20iOiIyMDIzLTAxLTA5VDE1OjQxOjUwWiIsImNyZWRlbnRpYWxTY2hlbWEiOnsiaWQiOiJodHRwczovL2FwaS5wcmVwcm9kLmVic2kuZXUvdHJ1c3RlZC1zY2hlbWFzLXJlZ2lzdHJ5L3YxL3NjaGVtYXMvMHhiNzdmODUxNmE5NjU2MzFiNGYxOTdhZDU0YzY1YTllMmY5OTM2ZWJmYjc2YmFlNDkwNmQzMzc0NGRiY2M2MGJhIiwidHlwZSI6IkZ1bGxKc29uU2NoZW1hVmFsaWRhdG9yMjAyMSJ9LCJjcmVkZW50aWFsU3ViamVjdCI6eyJpZCI6ImRpZDprZXk6ejZNa2lkazU0VW9QVmhQcWtXRXhuZ1JqM0NkMzNqQWROY0tmZDZlUGtyQ1pFYmpVIiwiY3VycmVudEFkZHJlc3MiOlsiMSBCb3VsZXZhcmQgZGUgbGEgTGliZXJ0w6ksIDU5ODAwIExpbGxlIl0sImRhdGVPZkJpcnRoIjoiMTk5My0wNC0wOCIsImZhbWlseU5hbWUiOiJET0UiLCJmaXJzdE5hbWUiOiJKYW5lIiwiZ2VuZGVyIjoiRkVNQUxFIiwibmFtZUFuZEZhbWlseU5hbWVBdEJpcnRoIjoiSmFuZSBET0UiLCJwZXJzb25hbElkZW50aWZpZXIiOiIwOTA0MDA4MDg0SCIsInBsYWNlT2ZCaXJ0aCI6IkxJTExFLCBGUkFOQ0UifSwiZXZpZGVuY2UiOlt7ImRvY3VtZW50UHJlc2VuY2UiOlsiUGh5c2ljYWwiXSwiZXZpZGVuY2VEb2N1bWVudCI6WyJQYXNzcG9ydCJdLCJzdWJqZWN0UHJlc2VuY2UiOiJQaHlzaWNhbCIsInR5cGUiOlsiRG9jdW1lbnRWZXJpZmljYXRpb24iXSwidmVyaWZpZXIiOiJkaWQ6ZWJzaToyQTlCWjlTVWU2QmF0YWNTcHZzMVY1Q2RqSHZMcFE3YkVzaTJKYjZMZEhLblF4YU4ifV19LCJqdGkiOiJ1cm46dXVpZDo3Y2YxOGE2MC1lNjI0LTQ0YjAtOGVhOC1hZWM2MzBhMmZiNjYifQ.no9qGLBMSR1tq9Svn0fNdI5B0gIlO6NrwQrnvd7tqVjnPdXvpeTR4E9TtI346Ba6YQllbrSh9-HFIa-uzDlzBA",
                    "properties": {
                        "evidence": [
                            {
                                "documentPresence": ["Physical"],
                                "evidenceDocument": ["Passport"],
                                "subjectPresence": "Physical",
                                "type": ["DocumentVerification"],
                                "verifier": "did:ebsi:2A9BZ9SUe6BatacSpvs1V5CdjHvLpQ7bEsi2Jb6LdHKnQxaN"
                            }
                        ]
                    },
                    "subjectId": "did:key:z6Mkidk54UoPVhPqkWExngRj3Cd33jAdNcKfd6ePkrCZEbjU",
                    "type": ["VerifiableCredential", "VerifiableAttestation", "VerifiableId"],
                    "validFrom": "2023-01-09T15:41:50Z"
                }
            ],
            "verification_result": {
                "policyResults": {
                    "SignaturePolicy": true,
                    "ChallengePolicy": true,
                    "PresentationDefinitionPolicy": true
                },
                "valid": true
            },
            "vp": {
                "holder": "did:key:z6Mkidk54UoPVhPqkWExngRj3Cd33jAdNcKfd6ePkrCZEbjU",
                "issuerId": "did:key:z6Mkidk54UoPVhPqkWExngRj3Cd33jAdNcKfd6ePkrCZEbjU",
                "subjectId": "did:key:z6Mkidk54UoPVhPqkWExngRj3Cd33jAdNcKfd6ePkrCZEbjU",
                "verifiableCredential": [
                    {
                        "context": [
                            {
                                "_isObject": false,
                                "properties": {},
                                "uri": "https://www.w3.org/2018/credentials/v1"
                            }
                        ],
                        "credentialSchema": {
                            "id": "https://api.preprod.ebsi.eu/trusted-schemas-registry/v1/schemas/0xb77f8516a965631b4f197ad54c65a9e2f9936ebfb76bae4906d33744dbcc60ba",
                            "properties": {
                                "id": "https://api.preprod.ebsi.eu/trusted-schemas-registry/v1/schemas/0xb77f8516a965631b4f197ad54c65a9e2f9936ebfb76bae4906d33744dbcc60ba"
                            },
                            "type": "FullJsonSchemaValidator2021"
                        },
                        "credentialSubject": {
                            "id": "did:key:z6Mkidk54UoPVhPqkWExngRj3Cd33jAdNcKfd6ePkrCZEbjU",
                            "properties": {
                                "currentAddress": ["1 Boulevard de la Liberté, 59800 Lille"],
                                "dateOfBirth": "1993-04-08",
                                "familyName": "DOE",
                                "firstName": "Jane",
                                "gender": "FEMALE",
                                "nameAndFamilyNameAtBirth": "Jane DOE",
                                "personalIdentifier": "0904008084H",
                                "placeOfBirth": "LILLE, FRANCE"
                            }
                        },
                        "id": "urn:uuid:7cf18a60-e624-44b0-8ea8-aec630a2fb66",
                        "issuanceDate": "2023-01-09T15:41:50Z",
                        "issued": "2023-01-09T15:41:50Z",
                        "issuer": {
                            "_isObject": false,
                            "id": "did:key:z6Mkvrfpsv1pB3qwEXJAWtz6u7zgib2PZYj5zRg8gJfCsJqv",
                            "properties": {}
                        },
                        "issuerId": "did:key:z6Mkvrfpsv1pB3qwEXJAWtz6u7zgib2PZYj5zRg8gJfCsJqv",
                        "jwt": "eyJraWQiOiJkaWQ6a2V5Ono2TWt2cmZwc3YxcEIzcXdFWEpBV3R6NnU3emdpYjJQWllqNXpSZzhnSmZDc0pxdiN6Nk1rdnJmcHN2MXBCM3F3RVhKQVd0ejZ1N3pnaWIyUFpZajV6Umc4Z0pmQ3NKcXYiLCJ0eXAiOiJKV1QiLCJhbGciOiJFZERTQSJ9.eyJzdWIiOiJkaWQ6a2V5Ono2TWtpZGs1NFVvUFZoUHFrV0V4bmdSajNDZDMzakFkTmNLZmQ2ZVBrckNaRWJqVSIsIm5iZiI6MTY3MzI3ODkxMCwiaXNzIjoiZGlkOmtleTp6Nk1rdnJmcHN2MXBCM3F3RVhKQVd0ejZ1N3pnaWIyUFpZajV6Umc4Z0pmQ3NKcXYiLCJpYXQiOjE2NzMyNzg5MTAsInZjIjp7InR5cGUiOlsiVmVyaWZpYWJsZUNyZWRlbnRpYWwiLCJWZXJpZmlhYmxlQXR0ZXN0YXRpb24iLCJWZXJpZmlhYmxlSWQiXSwiQGNvbnRleHQiOlsiaHR0cHM6Ly93d3cudzMub3JnLzIwMTgvY3JlZGVudGlhbHMvdjEiXSwiaWQiOiJ1cm46dXVpZDo3Y2YxOGE2MC1lNjI0LTQ0YjAtOGVhOC1hZWM2MzBhMmZiNjYiLCJpc3N1ZXIiOiJkaWQ6a2V5Ono2TWt2cmZwc3YxcEIzcXdFWEpBV3R6NnU3emdpYjJQWllqNXpSZzhnSmZDc0pxdiIsImlzc3VhbmNlRGF0ZSI6IjIwMjMtMDEtMDlUMTU6NDE6NTBaIiwiaXNzdWVkIjoiMjAyMy0wMS0wOVQxNTo0MTo1MFoiLCJ2YWxpZEZyb20iOiIyMDIzLTAxLTA5VDE1OjQxOjUwWiIsImNyZWRlbnRpYWxTY2hlbWEiOnsiaWQiOiJodHRwczovL2FwaS5wcmVwcm9kLmVic2kuZXUvdHJ1c3RlZC1zY2hlbWFzLXJlZ2lzdHJ5L3YxL3NjaGVtYXMvMHhiNzdmODUxNmE5NjU2MzFiNGYxOTdhZDU0YzY1YTllMmY5OTM2ZWJmYjc2YmFlNDkwNmQzMzc0NGRiY2M2MGJhIiwidHlwZSI6IkZ1bGxKc29uU2NoZW1hVmFsaWRhdG9yMjAyMSJ9LCJjcmVkZW50aWFsU3ViamVjdCI6eyJpZCI6ImRpZDprZXk6ejZNa2lkazU0VW9QVmhQcWtXRXhuZ1JqM0NkMzNqQWROY0tmZDZlUGtyQ1pFYmpVIiwiY3VycmVudEFkZHJlc3MiOlsiMSBCb3VsZXZhcmQgZGUgbGEgTGliZXJ0w6ksIDU5ODAwIExpbGxlIl0sImRhdGVPZkJpcnRoIjoiMTk5My0wNC0wOCIsImZhbWlseU5hbWUiOiJET0UiLCJmaXJzdE5hbWUiOiJKYW5lIiwiZ2VuZGVyIjoiRkVNQUxFIiwibmFtZUFuZEZhbWlseU5hbWVBdEJpcnRoIjoiSmFuZSBET0UiLCJwZXJzb25hbElkZW50aWZpZXIiOiIwOTA0MDA4MDg0SCIsInBsYWNlT2ZCaXJ0aCI6IkxJTExFLCBGUkFOQ0UifSwiZXZpZGVuY2UiOlt7ImRvY3VtZW50UHJlc2VuY2UiOlsiUGh5c2ljYWwiXSwiZXZpZGVuY2VEb2N1bWVudCI6WyJQYXNzcG9ydCJdLCJzdWJqZWN0UHJlc2VuY2UiOiJQaHlzaWNhbCIsInR5cGUiOlsiRG9jdW1lbnRWZXJpZmljYXRpb24iXSwidmVyaWZpZXIiOiJkaWQ6ZWJzaToyQTlCWjlTVWU2QmF0YWNTcHZzMVY1Q2RqSHZMcFE3YkVzaTJKYjZMZEhLblF4YU4ifV19LCJqdGkiOiJ1cm46dXVpZDo3Y2YxOGE2MC1lNjI0LTQ0YjAtOGVhOC1hZWM2MzBhMmZiNjYifQ.no9qGLBMSR1tq9Svn0fNdI5B0gIlO6NrwQrnvd7tqVjnPdXvpeTR4E9TtI346Ba6YQllbrSh9-HFIa-uzDlzBA",
                        "properties": {
                            "evidence": [
                                {
                                    "documentPresence": ["Physical"],
                                    "evidenceDocument": ["Passport"],
                                    "subjectPresence": "Physical",
                                    "type": ["DocumentVerification"],
                                    "verifier": "did:ebsi:2A9BZ9SUe6BatacSpvs1V5CdjHvLpQ7bEsi2Jb6LdHKnQxaN"
                                }
                            ]
                        },
                        "subjectId": "did:key:z6Mkidk54UoPVhPqkWExngRj3Cd33jAdNcKfd6ePkrCZEbjU",
                        "type": ["VerifiableCredential", "VerifiableAttestation", "VerifiableId"],
                        "validFrom": "2023-01-09T15:41:50Z"
                    }
                ],
                "challenge": "41676715-a069-4f43-baee-57bdbeaf2c98",
                "context": [
                    {
                        "_isObject": false,
                        "properties": {},
                        "uri": "https://www.w3.org/2018/credentials/v1"
                    }
                ],
                "id": "urn:uuid:a8d3b6fb-03ca-4550-8a95-f812d4ee8f8f",
                "jwt": "eyJraWQiOiJkaWQ6a2V5Ono2TWtpZGs1NFVvUFZoUHFrV0V4bmdSajNDZDMzakFkTmNLZmQ2ZVBrckNaRWJqVSN6Nk1raWRrNTRVb1BWaFBxa1dFeG5nUmozQ2QzM2pBZE5jS2ZkNmVQa3JDWkVialUiLCJ0eXAiOiJKV1QiLCJhbGciOiJFZERTQSJ9.eyJzdWIiOiJkaWQ6a2V5Ono2TWtpZGs1NFVvUFZoUHFrV0V4bmdSajNDZDMzakFkTmNLZmQ2ZVBrckNaRWJqVSIsIm5iZiI6MTY3NDA4Nzc2MCwiaXNzIjoiZGlkOmtleTp6Nk1raWRrNTRVb1BWaFBxa1dFeG5nUmozQ2QzM2pBZE5jS2ZkNmVQa3JDWkVialUiLCJ2cCI6eyJ0eXBlIjpbIlZlcmlmaWFibGVQcmVzZW50YXRpb24iXSwiQGNvbnRleHQiOlsiaHR0cHM6Ly93d3cudzMub3JnLzIwMTgvY3JlZGVudGlhbHMvdjEiXSwiaWQiOiJ1cm46dXVpZDphOGQzYjZmYi0wM2NhLTQ1NTAtOGE5NS1mODEyZDRlZThmOGYiLCJob2xkZXIiOiJkaWQ6a2V5Ono2TWtpZGs1NFVvUFZoUHFrV0V4bmdSajNDZDMzakFkTmNLZmQ2ZVBrckNaRWJqVSIsInZlcmlmaWFibGVDcmVkZW50aWFsIjpbImV5SnJhV1FpT2lKa2FXUTZhMlY1T25vMlRXdDJjbVp3YzNZeGNFSXpjWGRGV0VwQlYzUjZOblUzZW1kcFlqSlFXbGxxTlhwU1p6aG5TbVpEYzBweGRpTjZOazFyZG5KbWNITjJNWEJDTTNGM1JWaEtRVmQwZWpaMU4zcG5hV0l5VUZwWmFqVjZVbWM0WjBwbVEzTktjWFlpTENKMGVYQWlPaUpLVjFRaUxDSmhiR2NpT2lKRlpFUlRRU0o5LmV5SnpkV0lpT2lKa2FXUTZhMlY1T25vMlRXdHBaR3MxTkZWdlVGWm9VSEZyVjBWNGJtZFNhak5EWkRNemFrRmtUbU5MWm1RMlpWQnJja05hUldKcVZTSXNJbTVpWmlJNk1UWTNNekkzT0RreE1Dd2lhWE56SWpvaVpHbGtPbXRsZVRwNk5rMXJkbkptY0hOMk1YQkNNM0YzUlZoS1FWZDBlaloxTjNwbmFXSXlVRnBaYWpWNlVtYzRaMHBtUTNOS2NYWWlMQ0pwWVhRaU9qRTJOek15TnpnNU1UQXNJblpqSWpwN0luUjVjR1VpT2xzaVZtVnlhV1pwWVdKc1pVTnlaV1JsYm5ScFlXd2lMQ0pXWlhKcFptbGhZbXhsUVhSMFpYTjBZWFJwYjI0aUxDSldaWEpwWm1saFlteGxTV1FpWFN3aVFHTnZiblJsZUhRaU9sc2lhSFIwY0hNNkx5OTNkM2N1ZHpNdWIzSm5Mekl3TVRndlkzSmxaR1Z1ZEdsaGJITXZkakVpWFN3aWFXUWlPaUoxY200NmRYVnBaRG8zWTJZeE9HRTJNQzFsTmpJMExUUTBZakF0T0dWaE9DMWhaV00yTXpCaE1tWmlOallpTENKcGMzTjFaWElpT2lKa2FXUTZhMlY1T25vMlRXdDJjbVp3YzNZeGNFSXpjWGRGV0VwQlYzUjZOblUzZW1kcFlqSlFXbGxxTlhwU1p6aG5TbVpEYzBweGRpSXNJbWx6YzNWaGJtTmxSR0YwWlNJNklqSXdNak10TURFdE1EbFVNVFU2TkRFNk5UQmFJaXdpYVhOemRXVmtJam9pTWpBeU15MHdNUzB3T1ZReE5UbzBNVG8xTUZvaUxDSjJZV3hwWkVaeWIyMGlPaUl5TURJekxUQXhMVEE1VkRFMU9qUXhPalV3V2lJc0ltTnlaV1JsYm5ScFlXeFRZMmhsYldFaU9uc2lhV1FpT2lKb2RIUndjem92TDJGd2FTNXdjbVZ3Y205a0xtVmljMmt1WlhVdmRISjFjM1JsWkMxelkyaGxiV0Z6TFhKbFoybHpkSEo1TDNZeEwzTmphR1Z0WVhNdk1IaGlOemRtT0RVeE5tRTVOalUyTXpGaU5HWXhPVGRoWkRVMFl6WTFZVGxsTW1ZNU9UTTJaV0ptWWpjMlltRmxORGt3Tm1Rek16YzBOR1JpWTJNMk1HSmhJaXdpZEhsd1pTSTZJa1oxYkd4S2MyOXVVMk5vWlcxaFZtRnNhV1JoZEc5eU1qQXlNU0o5TENKamNtVmtaVzUwYVdGc1UzVmlhbVZqZENJNmV5SnBaQ0k2SW1ScFpEcHJaWGs2ZWpaTmEybGthelUwVlc5UVZtaFFjV3RYUlhodVoxSnFNME5rTXpOcVFXUk9ZMHRtWkRabFVHdHlRMXBGWW1wVklpd2lZM1Z5Y21WdWRFRmtaSEpsYzNNaU9sc2lNU0JDYjNWc1pYWmhjbVFnWkdVZ2JHRWdUR2xpWlhKMHc2a3NJRFU1T0RBd0lFeHBiR3hsSWwwc0ltUmhkR1ZQWmtKcGNuUm9Jam9pTVRrNU15MHdOQzB3T0NJc0ltWmhiV2xzZVU1aGJXVWlPaUpFVDBVaUxDSm1hWEp6ZEU1aGJXVWlPaUpLWVc1bElpd2laMlZ1WkdWeUlqb2lSa1ZOUVV4Rklpd2libUZ0WlVGdVpFWmhiV2xzZVU1aGJXVkJkRUpwY25Sb0lqb2lTbUZ1WlNCRVQwVWlMQ0p3WlhKemIyNWhiRWxrWlc1MGFXWnBaWElpT2lJd09UQTBNREE0TURnMFNDSXNJbkJzWVdObFQyWkNhWEowYUNJNklreEpURXhGTENCR1VrRk9RMFVpZlN3aVpYWnBaR1Z1WTJVaU9sdDdJbVJ2WTNWdFpXNTBVSEpsYzJWdVkyVWlPbHNpVUdoNWMybGpZV3dpWFN3aVpYWnBaR1Z1WTJWRWIyTjFiV1Z1ZENJNld5SlFZWE56Y0c5eWRDSmRMQ0p6ZFdKcVpXTjBVSEpsYzJWdVkyVWlPaUpRYUhsemFXTmhiQ0lzSW5SNWNHVWlPbHNpUkc5amRXMWxiblJXWlhKcFptbGpZWFJwYjI0aVhTd2lkbVZ5YVdacFpYSWlPaUprYVdRNlpXSnphVG95UVRsQ1dqbFRWV1UyUW1GMFlXTlRjSFp6TVZZMVEyUnFTSFpNY0ZFM1lrVnphVEpLWWpaTVpFaExibEY0WVU0aWZWMTlMQ0pxZEdraU9pSjFjbTQ2ZFhWcFpEbzNZMll4T0dFMk1DMWxOakkwTFRRMFlqQXRPR1ZoT0MxaFpXTTJNekJoTW1aaU5qWWlmUS5ubzlxR0xCTVNSMXRxOVN2bjBmTmRJNUIwZ0lsTzZOcndRcm52ZDd0cVZqblBkWHZwZVRSNEU5VHRJMzQ2QmE2WVFsbGJyU2g5LUhGSWEtdXpEbHpCQSJdfSwiaWF0IjoxNjc0MDg3NzYwLCJub25jZSI6IjQxNjc2NzE1LWEwNjktNGY0My1iYWVlLTU3YmRiZWFmMmM5OCIsImp0aSI6InVybjp1dWlkOmE4ZDNiNmZiLTAzY2EtNDU1MC04YTk1LWY4MTJkNGVlOGY4ZiJ9.RxFMgwvyTQkny0eCF2my016ff9yU3sUC8oOHARKRYMG7-u0He8sjjwB7cM0qIxVuNUbXfqAGm79UXIZGX4MzAg",
                "properties": {
                    "holder": "did:key:z6Mkidk54UoPVhPqkWExngRj3Cd33jAdNcKfd6ePkrCZEbjU",
                    "verifiableCredential": [
                        "eyJraWQiOiJkaWQ6a2V5Ono2TWt2cmZwc3YxcEIzcXdFWEpBV3R6NnU3emdpYjJQWllqNXpSZzhnSmZDc0pxdiN6Nk1rdnJmcHN2MXBCM3F3RVhKQVd0ejZ1N3pnaWIyUFpZajV6Umc4Z0pmQ3NKcXYiLCJ0eXAiOiJKV1QiLCJhbGciOiJFZERTQSJ9.eyJzdWIiOiJkaWQ6a2V5Ono2TWtpZGs1NFVvUFZoUHFrV0V4bmdSajNDZDMzakFkTmNLZmQ2ZVBrckNaRWJqVSIsIm5iZiI6MTY3MzI3ODkxMCwiaXNzIjoiZGlkOmtleTp6Nk1rdnJmcHN2MXBCM3F3RVhKQVd0ejZ1N3pnaWIyUFpZajV6Umc4Z0pmQ3NKcXYiLCJpYXQiOjE2NzMyNzg5MTAsInZjIjp7InR5cGUiOlsiVmVyaWZpYWJsZUNyZWRlbnRpYWwiLCJWZXJpZmlhYmxlQXR0ZXN0YXRpb24iLCJWZXJpZmlhYmxlSWQiXSwiQGNvbnRleHQiOlsiaHR0cHM6Ly93d3cudzMub3JnLzIwMTgvY3JlZGVudGlhbHMvdjEiXSwiaWQiOiJ1cm46dXVpZDo3Y2YxOGE2MC1lNjI0LTQ0YjAtOGVhOC1hZWM2MzBhMmZiNjYiLCJpc3N1ZXIiOiJkaWQ6a2V5Ono2TWt2cmZwc3YxcEIzcXdFWEpBV3R6NnU3emdpYjJQWllqNXpSZzhnSmZDc0pxdiIsImlzc3VhbmNlRGF0ZSI6IjIwMjMtMDEtMDlUMTU6NDE6NTBaIiwiaXNzdWVkIjoiMjAyMy0wMS0wOVQxNTo0MTo1MFoiLCJ2YWxpZEZyb20iOiIyMDIzLTAxLTA5VDE1OjQxOjUwWiIsImNyZWRlbnRpYWxTY2hlbWEiOnsiaWQiOiJodHRwczovL2FwaS5wcmVwcm9kLmVic2kuZXUvdHJ1c3RlZC1zY2hlbWFzLXJlZ2lzdHJ5L3YxL3NjaGVtYXMvMHhiNzdmODUxNmE5NjU2MzFiNGYxOTdhZDU0YzY1YTllMmY5OTM2ZWJmYjc2YmFlNDkwNmQzMzc0NGRiY2M2MGJhIiwidHlwZSI6IkZ1bGxKc29uU2NoZW1hVmFsaWRhdG9yMjAyMSJ9LCJjcmVkZW50aWFsU3ViamVjdCI6eyJpZCI6ImRpZDprZXk6ejZNa2lkazU0VW9QVmhQcWtXRXhuZ1JqM0NkMzNqQWROY0tmZDZlUGtyQ1pFYmpVIiwiY3VycmVudEFkZHJlc3MiOlsiMSBCb3VsZXZhcmQgZGUgbGEgTGliZXJ0w6ksIDU5ODAwIExpbGxlIl0sImRhdGVPZkJpcnRoIjoiMTk5My0wNC0wOCIsImZhbWlseU5hbWUiOiJET0UiLCJmaXJzdE5hbWUiOiJKYW5lIiwiZ2VuZGVyIjoiRkVNQUxFIiwibmFtZUFuZEZhbWlseU5hbWVBdEJpcnRoIjoiSmFuZSBET0UiLCJwZXJzb25hbElkZW50aWZpZXIiOiIwOTA0MDA4MDg0SCIsInBsYWNlT2ZCaXJ0aCI6IkxJTExFLCBGUkFOQ0UifSwiZXZpZGVuY2UiOlt7ImRvY3VtZW50UHJlc2VuY2UiOlsiUGh5c2ljYWwiXSwiZXZpZGVuY2VEb2N1bWVudCI6WyJQYXNzcG9ydCJdLCJzdWJqZWN0UHJlc2VuY2UiOiJQaHlzaWNhbCIsInR5cGUiOlsiRG9jdW1lbnRWZXJpZmljYXRpb24iXSwidmVyaWZpZXIiOiJkaWQ6ZWJzaToyQTlCWjlTVWU2QmF0YWNTcHZzMVY1Q2RqSHZMcFE3YkVzaTJKYjZMZEhLblF4YU4ifV19LCJqdGkiOiJ1cm46dXVpZDo3Y2YxOGE2MC1lNjI0LTQ0YjAtOGVhOC1hZWM2MzBhMmZiNjYifQ.no9qGLBMSR1tq9Svn0fNdI5B0gIlO6NrwQrnvd7tqVjnPdXvpeTR4E9TtI346Ba6YQllbrSh9-HFIa-uzDlzBA"
                    ]
                },
                "type": ["VerifiablePresentation"]
            }
        }
    ]
}
```

(Empty values that are not set \[= null] are omitted for this example, e.g. no expirationDate was set, thus you'll receive `"expirationDate": null`)
