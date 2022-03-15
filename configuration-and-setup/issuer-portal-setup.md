# Issuer portal setup

To setup the issuer portal backend a few things may need to be considered and some configuration may be required, depending on your situation.

## Data root

Since the issuer portal backend is implemented in the same service as the wallet backend, refer to the wallet backend setup section for [**details**](../configuration-and-setup/wallet-backend-setup.md#data-root).

## Issuer portal backend configuration

The configuration of the issuer portal backend can be adapted, by modifying the file 

`config/issuer-config.json`

### External URLs

Configure the URLs via which the issuer portal UI and backend API will be reachable from an external source, e.g. a web browser, or the wallet backend requesting credential issuance:

```
{
  "issuerUiUrl": "https://issuer.waltid.org",
  "issuerApiUrl": "https://issuer.waltid.org/issuer-api",
[...]
}
```

### Issuer DID

Configure the DID used to sign issued credentials. Refer to [**Initializing issuer portal backend**](#initializing-issuer-portal-backend) for options to initialize a DID for the backend.

By default, the issuer backend will choose the first available DID in its data store. To enforce, which DID to use, set it in the **issuer-config.json** like so:

```
{
  [...]
  "issuerDid": "did:key:z6MkpooraZxqm99KwnMiRMzrVibVN5o5ow1BesLGFaG953RS",
  [...]
}
```

### Known wallets

The issuer portal supports an issuance flow, that is triggered directly from the issuer portal into the SSI wallet. To support this kind of issuance flow, you need to configure the known wallets and how to connect to them:

This example shows the known wallet configuration for the walt.id web wallet, in **issuer-config.json**:

```
{
[...]
  "wallets": {
    "walt.id": {
      "id": "walt.id",
      "url": "https://wallet.waltid.org",
      "presentPath": "api/wallet/siopv2/initPresentation/",
      "receivePath" : "api/wallet/siopv2/initPassiveIssuance/",
      "description": "walt.id web wallet"
    }
  }
}
```

### Configuration example

Here's a complete example for the **issuer-config.json**:

```
{
  "issuerUiUrl": "https://issuer.waltid.org",
  "issuerApiUrl": "https://issuer.waltid.org/issuer-api",
  "issuerDid": "did:key:z6MkpooraZxqm99KwnMiRMzrVibVN5o5ow1BesLGFaG953RS",
  "wallets": {
    "walt.id": {
      "id": "walt.id",
      "url": "https://wallet.waltid.org",
      "presentPath": "api/wallet/siopv2/initPresentation/",
      "receivePath" : "api/wallet/siopv2/initPassiveIssuance/",
      "description": "walt.id web wallet"
    }
  }
}
```

## Issuer portal backend data storage

Data, such as dids and keys, are currently stored in the **issuer** subfolder, like so:

`<data_root>/data/issuer`

In the future various options to configure the issuer data storage may be provided.

## Binding address and port

Since the issuer portal backend is implemented in the same service as the wallet backend, refer to the wallet backend setup section for [**details**](../configuration-and-setup/wallet-backend-setup.md#binding-address-and-port).


## Initializing issuer portal backend

To set up an issuer portal backend, it is crucial to define the DID by which issued credentials should be signed.

The wallet backend intergrates a subset of commands from the **walt.id SSI Kit**, to accomplish simple **key and DID management**.

{% hint style="info" %}
For simplicity the examples will use the **command placeholder**

**`waltid-wallet-backend`**

The actual command depends on your execution environment, in the case of the docker container this could translate to something like:

`docker run -p 8080:8080 -e WALTID_DATA_ROOT=/data -v $PWD:/data waltid/ssikit-wallet-backend`
{% endhint %}

To manage keys and dids for the issuer, use the **config** command, with the command flag _--as-issuer_, or its shortcut _-i_.

_E.g._:

```
waltid-wallet-backend config --as-issuer --help
```

### Setting up an issuer DID

The following examples show typical use cases and scenarios of setting up an issuer backend for various ecosystems.

#### EBSI/ESSIF anchored issuer DID

1) **Create a new Secp256k1 key:**

```
waltid-wallet-backend config --as-issuer key gen -a Secp256k1
```

_Sample output_

```
[...]
Key "528435baadfd49559b1fe141f43bd258" generated.
```

2) **Create a new _did:ebsi_:**

```
waltid-wallet-backend config --as-issuer did create -m ebsi -k 528435baadfd49559b1fe141f43bd258
```

_Sample output_

```
[...]
DID created: did:ebsi:zetpTbH5RwCcQVAfAXGFKyF
[...]
```

3) **Register the DID on the EBSI blockchain:**

Get the **bearer token** from `https://app.preprod.ebsi.eu/users-onboarding/`, and then execute these commands:

```
echo "[bearer-token from above mentioned onboarding page]" > bearer-token.txt

waltid-wallet-backend config --as-issuer essif onboard --did did:ebsi:zetpTbH5RwCcQVAfAXGFKyF bearer-token.txt

waltid-wallet-backend config --as-issuer essif auth-api --did did:ebsi:zetpTbH5RwCcQVAfAXGFKyF

waltid-wallet-backend config --as-issuer essif did register --did did:ebsi:zetpTbH5RwCcQVAfAXGFKyF
```

4) **Set the _issuerDid_ config property**

Also refer to [Issuer DID configuration](#issuer-did).

_issuer-config.json_
```
{
  [...]
  "issuerDid": "did:ebsi:zetpTbH5RwCcQVAfAXGFKyF",
  [...]
}
```

#### DNS/Web anchored issuer DID

1) **Create a new _did:web_**

Run the following command, **replacing the _domain_ (-d) and _path_ (-p) arguments**, matching your web server on which you can publish the did document:

```
waltid-wallet-backend config --as-issuer did create -m web -d "walt.id" -p "my-issuer"
```

Observe the command output:

```
[...]
Results:

DID created: did:web:walt.id:my-issuer

DID document (below, JSON):

{
    "assertionMethod" : [
        "did:web:walt.id:my-issuer#f72884caa5d641aca30353ce65b2bc07"
    ],
    "authentication" : [
        "did:web:walt.id:my-issuer#f72884caa5d641aca30353ce65b2bc07"
    ],
    "@context" : "https://www.w3.org/ns/did/v1",
    "id" : "did:web:walt.id:my-issuer",
    "verificationMethod" : [
        {
            [...]
        }
    ]
}

Install this did:web at: https://walt.id/.well-known/my-issuer/did.json
```

2) **Publish the DID document on the web server:**

Copy the DID document from the above command output, on the path printed by that same command. The DID document **in this example** should be resolvable from this URL:

`https://walt.id/.well-known/my-issuer/did.json`

_The **domain** and **path** will be different in your case._

3) **Set the _issuerDid_ config property**

Also refer to [Issuer DID configuration](#issuer-did).

_issuer-config.json_
```
{
  [...]
  "issuerDid": "did:web:walt.id:my-issuer",
  [...]
}
```

#### Importing a DID and Key using SSI Kit

1) Export the DID and Key from SSI Kit

```
```

2) Import the DID and Key in the issuer backend

```
```

3) **Set the _issuerDid_ config property**

Also refer to [Issuer DID configuration](#issuer-did).