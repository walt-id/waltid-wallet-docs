# Verifier portal setup

To setup the verifier portal backend a few things may need to be considered and some configuration may be required, depending on your situation.

## Data root

Since the verifier portal backend is implemented in the same service as the wallet backend, refer to the wallet backend setup section for [**details**](../configuration-and-setup/wallet-backend-setup.md#data-root).

## Verifier portal backend configuration

The configuration of the verifier portal backend can be adapted, by modifying the file 

`config/verifier-config.json`

### External URLs

Configure the URLs via which the verifier portal UI and backend API will be reachable from an external source, e.g. a web browser, or the wallet backend responding to a credential presentation request:

```
{
  "verifierUiUrl": "https://verifier.waltid.org",
  "verifierApiUrl": "https://verifier.waltid.org/verifier-api",
[...]
}
```

### Known wallets

The verifier portal supports the OIDC/SIOPv2 credential presentation flow, that is triggered directly from the verifier portal requesting credentials from an SSI wallet. To support this kind of verification flow, you need to configure the known wallets and how to connect to them:

This example shows the known wallet configuration for the walt.id web wallet, in **verifier-config.json**:

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

Here's a complete example for the **verifier-config.json**:

```
{
  "verifierUiUrl": "https://verifier.waltid.org",
  "verifierApiUrl": "https://verifier.waltid.org/verifier-api",
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

## Verifier portal backend data storage

Data, such as dids and keys, are currently stored in the **verifier** subfolder, like so:

`<data_root>/data/verifier`

In the future various options to configure the verifier data storage may be provided.

## Binding address and port

Since the verifier portal backend is implemented in the same service as the wallet backend, refer to the wallet backend setup section for [**details**](../configuration-and-setup/wallet-backend-setup.md#binding-address-and-port).