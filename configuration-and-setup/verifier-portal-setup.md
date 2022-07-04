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

### Verification policies

By default, the verifier backend will apply some basic verification policies to the verifiable presentations and credentials, presented by the wallet.
The default verification policies are:

* **SignaturePolicy**
  * _checks the signature of the credential_
* **ChallengePolicy**
  * _verifies that the credential was signed against the challenge requested by the verifier backend_
* **VpTokenClaimPolicy**
  * _verifies that the SIOP response (presentation from the wallet) matches the vp_token claim_

In order to enforce other policies to be executed on the presented credentials, you can use the `additionalPolicies` property in the verifier configuration.

This also allows for configuring custom dynamic policies, and supports specification of policy arguments.

The verifier backend supports the verification policy management commands provided by the SSIKit, using the following CLI subcommand: 

```
waltid-walletkit config --as-verifier vc policies --help
```

For details of using verification policies and creating custom dynamic policies, refer to the corresponding section of the [SSIKit documentation](https://docs.walt.id/v/ssikit/concepts/verification-policies).


This example shows the configuration of additional policies in **verifier-config.json**:
```
{
  [...]
  "additionalPolicies": [
    {
      "policy": "JsonSchemaPolicy"
    },
    {
      "policy": "MyCustomPolicy",
      "argument": {
        "param": "value"
      }
    }
  ]
}
```



### Configuration example

Here's a complete example for the **verifier-config.json**:

```
{
  "verifierUiUrl": "https://verifier.waltid.org",
  "verifierApiUrl": "https://verifier.waltid.org/verifier-api",
  "additionalPolicies": [
    {
      "policy": "JsonSchemaPolicy"
    }
  ],
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