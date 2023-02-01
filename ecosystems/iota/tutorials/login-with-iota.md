---
description: Learn how to build a login with IOTA use-case
---

# Login With IOTA

In this tutorial, you will learn how to configure the wallet kit + other needed frontend projects to build a Login With IOTA use-case. The tutorial is divided into two sections. First, we will set up and configure the backend handling the business logic, followed by the frontend projects which are provided for an easy and fast lauch.

## Setting up the backend

### Cloning the Wallet-Kit

Make sure you have Docker installed on your machine

{% tabs %}
{% tab title="Docker Build" %}
1. Cloning the project

```
git clone https://github.com/walt-id/waltid-walletkit.git
```

2\. Change directory

```
cd waltid-walletkit
```

3\. Running the project

```
docker run -p 8080:8080 -e WALTID_DATA_ROOT=/data -v $PWD:/data waltid/walletkit run
```
{% endtab %}
{% endtabs %}

### Create the IOTA policy

To make sure only Verifiable Credentials with a "did:iota" can pass the verification (login), we will create a policy and use it in our verifier configuration.

**IOTA policy**

```json
{
"name": "DidIotaPolicy",
"policy": "package system\r\ndefault main = false\r\nmain { startswith(data[\"id\"], \"did:iota:\") }",
"input": {}
}
```

**Registering the policy**

The Wallet-Kit has multi tenant support, which means multiple customers, or tenants, can use a single instance of the wallet-kit, with each tenant having their own data and configurations isolated from others. And we will now create + use our IOTA tenant to register the policy

{% tabs %}
{% tab title="CURL" %}
**POST** /verifier-api/{tenantId}/config/policies/create/{name}\
\
Use swagger to [make the request](http://localhost:8080/api/swagger#/Verifier%20Configuration/createDynamicPolicy)



```bash
curl -X 'POST' \
  'http://localhost:8080/verifier-api/iota/config/templates/DidIotaPolicy' \
  -H 'accept: text/plain' \
  -H 'Content-Type: application/json' \
  -d 'SEE EXAMPLE BODY'
```
{% endtab %}

{% tab title="Example Body" %}
```json
{
"name": "DidIotaPolicy",
"policy": "package system\r\ndefault main = false\r\nmain { startswith(data[\"id\"], \"did:iota:\") }",
"input": {}
}
```
{% endtab %}
{% endtabs %}

### Create the IOTA Tenant + configuration

The Wallet-Kit has multi tenant support, which means multiple customers, or tenants, can use a single instance of the wallet-kit, with each tenant having their own data and configurations isolated from others. And we will now create our IOTA tenant to handle all IOTA related operations.

First, we get the configuration of the default tenant and use it as a baseline for the configuration of our IOTA tenant.

{% tabs %}
{% tab title="CURL" %}
**GET** /verifier-api/{tenantId}/config/getConfiguration\
\
Use swagger to [make the request](http://localhost:8080/api/swagger#/Verifier%20Configuration/getConfiguration)\


```bash
curl -X 'GET' \
  'http://localhost:8080/verifier-api/default/config/getConfiguration' \
  -H 'accept: application/json'
```
{% endtab %}

{% tab title="Response Example" %}
```json
{
  "additionalPolicies": [],
  "allowedWebhookHosts": [
    "http://localhost",
    "http://wallet.local"
  ],
  "verifierApiUrl": "http://localhost:8080/verifier-api/default",
  "verifierUiUrl": "http://localhost:4000",
  "wallets": {
    "walt.id": {
      "description": "walt.id web wallet",
      "id": "walt.id",
      "presentPath": "api/siop/initiatePresentation/",
      "receivePath": "api/siop/initiateIssuance/",
      "url": "http://localhost:3000"
    },
    "local": {
      "description": "local wallet",
      "id": "local",
      "presentPath": "api/siop/initiatePresentation/",
      "receivePath": "api/siop/initiateIssuance/",
      "url": "http://localhost:3000"
    }
  }
}
```
{% endtab %}
{% endtabs %}

Using the response, we just got from the default tenant configuration, we will update `additionalPolicies` as well as the `verifierApiUrl` as follows:

<pre class="language-json"><code class="lang-json">{
  <a data-footnote-ref href="#user-content-fn-1">"additionalPolicies": [{ "policy": "DidIotaPolicy" }],</a>
  "allowedWebhookHosts": [
    "http://localhost",
    "http://wallet.local"
  ],
  <a data-footnote-ref href="#user-content-fn-2">"verifierApiUrl": "http://localhost:8080/verifier-api/iota",</a>
  "verifierUiUrl": "http://localhost:4000",
  "wallets": {
    "walt.id": {
      "description": "walt.id web wallet",
      "id": "walt.id",
      "presentPath": "api/siop/initiatePresentation/",
      "receivePath": "api/siop/initiateIssuance/",
      "url": "http://localhost:3000"
    },
    "local": {
      "description": "local wallet",
      "id": "local",
      "presentPath": "api/siop/initiatePresentation/",
      "receivePath": "api/siop/initiateIssuance/",
      "url": "http://localhost:3000"
    }
  }
}
</code></pre>

Let's now set the above configuration object as the configuration for our IOTA tenant.

{% tabs %}
{% tab title="CURL" %}
**POST** /verifier-api/{tenantId}/config/setConfiguration\
\
Use swagger to [make the request](http://localhost:8080/api/swagger#/Verifier%20Configuration/getConfiguration)\


```bash
curl -X 'POST' \
  'http://localhost:8080/verifier-api/iota/config/setConfiguration' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d 'SEE EXAMPLE BODY'
```
{% endtab %}

{% tab title="Example Body" %}
```json
{
  
"additionalPolicies": [{ "policy": "DidIotaPolicy" }],


  "allowedWebhookHosts": [
    "http://localhost",
    "http://wallet.local"
  ],
  
"verifierApiUrl": "http://localhost:8080/verifier-api/iota",


  "verifierUiUrl": "http://localhost:4000",
  "wallets": {
    "walt.id": {
      "description": "walt.id web wallet",
      "id": "walt.id",
      "presentPath": "api/siop/initiatePresentation/",
      "receivePath": "api/siop/initiateIssuance/",
      "url": "http://localhost:3000"
    },
    "local": {
      "description": "local wallet",
      "id": "local",
      "presentPath": "api/siop/initiatePresentation/",
      "receivePath": "api/siop/initiateIssuance/",
      "url": "http://localhost:3000"
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Setting up the frontend

To have the fully working Login With IOTA Demo up and running, we need three more projects:

1. The Waltid-Wallet - To create an IOTA DID and get a Verifiable Credential based on an IOTA DID issued
2. The Waltid-Issuer-Portal - The application responsible for issuing a Verifiable Credential to the DID IOTA holder (Wallet)
3. The Waltid-Verifier-Portal - The application in which the holder will log in to using a Verifiable Credential based on a DID IOTA

### Wallet

Make sure you have node.js installed on your machine

{% tabs %}
{% tab title="Local Build" %}
1. Cloning the project

```
git clone https://github.com/walt-id/waltid-web-wallet.git
```

2\. Change directory

```
cd waltid-web-wallet
```

3\. Install dependencies

```
yarn install
```

4\. Run the project

```
yarn dev
```
{% endtab %}
{% endtabs %}

### Issuer Portal

{% tabs %}
{% tab title="Local Build" %}
1. Cloning the project

```
git clone https://github.com/walt-id/waltid-issuer-portal.git
```

2\. Change directory

```
cd waltid-issuer-portal
```

3\. Install dependencies

```
yarn install
```

4\. Run the project

```
yarn dev
```
{% endtab %}
{% endtabs %}

### Verifier Portal

{% tabs %}
{% tab title="Local Build" %}
1. Cloning the project

```
git clone https://github.com/walt-id/waltid-verifier-portal.git
```

2\. Change directory

```
cd waltid-verifier-portal
```

3\. Checkout feat-iota brunch

```
git checkout feat-iota
```

4\. Install dependencies

```
yarn install
```

5\. Run the project

```
yarn dev
```
{% endtab %}
{% endtabs %}

### Let's log in with IOTA

Now that we have the backend and all frontend applications up and running, we can start the Login with IOTA flow.

1. **Creating a "did:iota" and registering it on the IOTA tangle.** Thankfully, the Wallet frontend, which we can now reach under [localhost:3000](http://localhost:3000/), already implements that logic, so we can easily open the settings, view Ecosystems, choose IOTA and register our DID. This will send a request to our running wallet-kit instance requesting the creation of DID as well as and registration of it with the IOTA tangle.&#x20;

{% hint style="info" %}
Make sure you **set the newly created IOTA DID as default DID of the wallet** in the ecosystem overview as shown in the video
{% endhint %}

{% embed url="https://www.loom.com/share/db161bbf650340af81d037025baa5c0e" %}
Creating + Registering an IOTA DID
{% endembed %}

2. **Claiming a VerifiableID Credential.** Visit [http:localhost:3000](http://localhost:3000/) where our walt-id-web-wallet is running and issue yourself a VerifiableId by clicking the "Request Credential" button and selecting the Walt.id Issuer portal (the other frontend we spun up earlier) running on [localhost:8000](http://localhost:8000/). Now what we have the Verifiable Credential, we can use it and login to the application.

{% embed url="https://www.loom.com/share/d0504af505de4e3bba0302643494df31" %}
Claiming a VerifiableID Credential
{% endembed %}

3. **Login into the Verifier Portal.** Visit [http:localhost:4000](http://localhost:4000/) where our walt-id-verifer-portal is running and click the "Login With IOTA" button to start the login flow. In background, the verifier portal will call our running instance of the wallet-kit, requesting a verification of the present credential. Using our custom policy from earlier, it will check that the subject of the present Verifiable Credential is an "iota:did". On the result screen of the verification, you will see a list of all the policies which were validated and if the validation has been successful.

{% embed url="https://www.loom.com/share/fbf0b7391aa94554acff1116c9cc1ebf" %}



[^1]: 

[^2]: 
