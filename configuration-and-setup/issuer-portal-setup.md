# Issuer portal setup

To setup the issuer portal backend a few things may need to be considered and some configuration may be required, depending on your situation.

## Data root

Since the issuer portal backend is implemented in the same service as the wallet backend, refer to the wallet backend setup section for [**details**](wallet-backend-setup.md#data-root).

## Issuer portal backend configuration

The configuration of the issuer portal backend can be adapted, by modifying the file

`config/issuer-config.json`

### External URLs

Configure the URLs via which the issuer portal UI and backend API will be reachable from an external source, e.g. a web browser, or the wallet backend requesting credential issuance:

```
{
  "issuerUiUrl": "https://issuer.walt-test.cloud",
  "issuerApiUrl": "https://issuer.walt-test.cloud/issuer-api",
[...]
}
```

### Issuer DID

Configure the DID used to sign issued credentials. Refer to [**Initializing issuer portal backend**](issuer-portal-setup.md#initializing-issuer-portal-backend) for options to initialize a DID for the backend.

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
      "url": "https://wallet.walt-test.cloud",
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
  "issuerUiUrl": "https://issuer.walt-test.cloud",
  "issuerApiUrl": "https://issuer.walt-test.cloud/issuer-api",
  "issuerDid": "did:key:z6MkpooraZxqm99KwnMiRMzrVibVN5o5ow1BesLGFaG953RS",
  "wallets": {
    "walt.id": {
      "id": "walt.id",
      "url": "https://wallet.walt-test.cloud",
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

Since the issuer portal backend is implemented in the same service as the wallet backend, refer to the wallet backend setup section for [**details**](wallet-backend-setup.md#binding-address-and-port).

## Initializing issuer portal backend

To set up an issuer portal backend, it is crucial to define the DID by which issued credentials should be signed.

The wallet backend intergrates a subset of commands from the **walt.id SSI Kit**, to accomplish simple **key and DID management**.

{% hint style="info" %}
For simplicity the examples will use the **command placeholder**

**`waltid-walletkit`**

The actual command depends on your execution environment, in the case of the docker container this could translate to something like:

`docker run -p 8080:8080 -e WALTID_DATA_ROOT=/data -v $PWD:/data waltid/walletkit`
{% endhint %}

To manage keys and dids for the issuer, use the **config** command, with the command flag _--as-issuer_, or its shortcut _-i_:

```
waltid-walletkit config --as-issuer --help
```

The following examples show typical use cases and scenarios of setting up an issuer backend for various ecosystems.

### EBSI/ESSIF anchored issuer DID

**Create a new \_Secp256k1**\_\*\* key\*\*

```
waltid-walletkit config --as-issuer key gen -a Secp256k1
```

_Sample output_

```
[...]
Key "528435baadfd49559b1fe141f43bd258" generated.
```

**Create a new \_did:ebsi**\_

```
waltid-walletkit config --as-issuer did create -m ebsi -k 528435baadfd49559b1fe141f43bd258
```

_Sample output_

```
[...]
DID created: did:ebsi:zetpTbH5RwCcQVAfAXGFKyF
[...]
```

**Register the DID on the EBSI blockchain**

Get the **bearer token** from [`https://app-pilot.ebsi.eu/users-onboarding/v2`](https://app-pilot.ebsi.eu/users-onboarding/v2), and then execute these commands:

```
echo "[bearer-token from above mentioned onboarding page]" > bearer-token.txt

waltid-walletkit config --as-issuer essif onboard --did did:ebsi:zetpTbH5RwCcQVAfAXGFKyF bearer-token.txt
waltid-walletkit config --as-issuer essif auth-api --did did:ebsi:zetpTbH5RwCcQVAfAXGFKyF
waltid-walletkit config --as-issuer essif did register --did did:ebsi:zetpTbH5RwCcQVAfAXGFKyF
```

**Set the \_issuerDid**\_\*\* config property\*\*

_issuer-config.json_

```
{
  [...]
  "issuerDid": "did:ebsi:zetpTbH5RwCcQVAfAXGFKyF",
  [...]
}
```

_Also refer to_ [_Issuer DID configuration_](issuer-portal-setup.md#issuer-did)

### DNS/Web anchored issuer DID

**Create a new \_did:web**\_

Run the following command, **replacing the \_domain**_\*\* (-d) and \*\*_**path**\_\*\* (-p) arguments\*\*, matching your web server on which you can publish the did document:

```
waltid-walletkit config --as-issuer did create -m web -d "walt.id" -p "my-issuer"
```

_Observe the command output:_

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

**Publish the DID document on the web server**

Copy the DID document from the above command output, and publish it on your web server, on the path printed by that same command.

The DID document **in this example** should be resolvable from this URL:

`https://walt.id/.well-known/my-issuer/did.json`

_The **domain** and **path** will be different in your case._

**Set the \_issuerDid**\_\*\* config property\*\*

_issuer-config.json_

```
{
  [...]
  "issuerDid": "did:web:walt.id:my-issuer",
  [...]
}
```

_Also refer to_ [_Issuer DID configuration_](issuer-portal-setup.md#issuer-did)

### Importing an external DID and key

If you want to use an existing DID, that you own, for issuance, you can import it, given that you have access to the associated private key and DID document. If the DID is resolvable through the standard mechanism of the given DID method, only the private key is required.

The private key should be available in **JWK or PEM format**.

**Import private key from JWK file**

In this example, we import a private key from the file _priv.jwk_:

```
waltid-walletkit config --as-issuer key import priv.jwk
```

_Output_

```
[...]
Results:

Key "e18e5427f7da48ce813e27ab3e5f66ad" imported.
```

Now we can import the DID, either by importing the DID document from a local JSON file, OR by resolving it from a public registry or likewise, depending on the DID method.

**Option 1: Resolve and import DID**

In this example, we import a **did:key**, for which the DID document can be derived without external DID registry, and associate it with the previously imported key ID:

```
waltid-walletkit config --as-issuer did import -k e18e5427f7da48ce813e27ab3e5f66ad -d did:key:z6MkovU6u4EpvADNVtxL21T9ocYzK8BDKyXtArskfbZkGsNe
```

**Option 2: Import DID document**

In case, the DID document cannot be resolved or derived, we can also import the DID document from a local JSON file:

```
waltid-walletkit config --as-issuer did import -k e18e5427f7da48ce813e27ab3e5f66ad -f /path/to/did.json
```

The relevant output for _**both import options**_, will look similar to this:

_Output_

```
[...]
DID imported: did:key:z6MkovU6u4EpvADNVtxL21T9ocYzK8BDKyXtArskfbZkGsNe
```

**Set the \_issuerDid**\_\*\* config property\*\*

_issuer-config.json_

```
{
  [...]
  "issuerDid": "did:key:z6MkovU6u4EpvADNVtxL21T9ocYzK8BDKyXtArskfbZkGsNe",
  [...]
}
```

_Also refer to_ [_Issuer DID configuration_](issuer-portal-setup.md#issuer-did)
