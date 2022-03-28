# Issuer & Verifier Portals

Besides the functionality outlined in [Functionality](../what-is-the-wallet/wallet/functional-overview/functionality.md), the wallet backend also features API backends for the **issuer and verifier portals**.

The issuer and verifier portals are **demo web portals** to showcase the scenarios of getting verifiable credentials issued into the wallet by a certified issuer or presenting a credential to a relying party. They can be used as reference implementations for issuers and verifiers to implement their own service platforms.

For credential and presentation exchange, we make use of the OIDC/SIOP protocols described in [OIDC](../concepts/oidc.md).

## Functional overview

### Verifier portal

* **Verifier web portal**
  * Web based user interface for requesting credential presentations through the web wallet
* **Wallet configuration**
  * Possibility to configure list of supported wallets (defaults to walt.id web wallet)
* **Presentation exchange**
  * Support for presentation exchange based on OIDC for verifiable presentations (SIOP) specification

### Issuer portal

* **Issuer web portal**
  * Web based user interface for issuing credentials to the web wallet
* **Wallet configuration**
  * Possibility to configure list of supported wallets (defaults to walt.id web wallet)
* **Credential issuance**
  * Support for issuing verifiable credentials to the web wallet, based on OIDC for credential issuance specification

## Architecture

Like the wallet-backend, the issuer and verifier portals are built on top of the SSI Kit, to leverage its functionality for issuing, signing and verifying credentials, provided by the Signatory and Auditor components resepectively.

Since the issuer and verifier backends are currently integrated with the wallet-backend project, refer to the [Architecture](../what-is-the-wallet/wallet/architecture.md) section for details.

To save you the roundtrip, in case you ended up on this page, here's the architecture diagram once more:

![Architecture](../what-is-the-wallet/wallet/architecture.png)
