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

## Issuer portal backend data storage

Data, such as dids and keys, are currently stored in the **issuer** subfolder, like so:

`<data_root>/data/issuer`

In the future various options to configure the issuer data storage may be provided.

## Binding address and port

Since the issuer portal backend is implemented in the same service as the wallet backend, refer to the wallet backend setup section for [**details**](../configuration-and-setup/wallet-backend-setup.md#binding-address-and-port).


## Initializing issuer portal backend

By specifying the optional startup parameter **--init-issuer** the issuer backend can be initialized with a DID used for credential issuance.

Assuming we will use docker to run the issuer backend, we can e.g. initialize the backend in line with the EBSI/ESSIF ecosystem, and create a **did:ebsi** like so:

```
cd docker
docker pull waltid/ssikit-wallet-backend
docker run -it -v $PWD:/waltid-wallet-backend/data-root -e WALTID_DATA_ROOT=./data-root waltid/ssikit-wallet-backend --init-issuer

# For the DID-method enter: "ebsi"
# For the bearer token copy/paste the value from: https://app.preprod.ebsi.eu/users-onboarding
```

The initialization routine will output the DID, which it registered on the EBSI/ESSIF ecosystem.

**Example output**

```
[...]
Issuer DID created and registered: did:ebsi:zmJvQzLBcitrEuJPC7AXMQs
```

Now copy the created DID and paste it in the **issuer-config.json**, in the **issuerDid** property, like described in the [**Issuer DID**](#issuer-did) configuration section.