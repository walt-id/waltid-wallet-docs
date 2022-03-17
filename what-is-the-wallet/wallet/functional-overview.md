# Wallet 101

The Wallet is a custodial wallet that enables end users to manage their keys, Decentralized Identifiers (DIDs) and Verifiable Credentials/Presentations (VCs, VPs).

The Wallet provides storage and management of SSI data, as well as, an integration of credential exchange protocols (e.g. OIDC, SIOP) for credential issuance and presentation exchange.

We provide

* Wallet backend service, implementing the business logic and RESTful APIs: [waltid-wallet-backend](https://github.com/walt-id/waltid-wallet-backend)
* Web Wallet frontend, implementing the user experience: [waltid-web-wallet](https://github.com/walt-id/waltid-web-wallet)

Additionally, we provide reference implementations of verifier and issuer web portals, showcasing the credential issuance and credential presentation flows: [waltid-issuer-portal](https://github.com/walt-id/waltid-issuer-portal) and [waltid-verifier-portal](https://github.com/walt-id/waltid-verifier-portal), where the backend business logic and APIs of the verifier and issuer portals are integrated in the _wallet-backend_ service.

## Project sources

* **Web wallet frontend:** [waltid-web-wallet](https://github.com/walt-id/waltid-web-wallet)
* **Wallet API backend** [waltid-wallet-backend](https://github.com/walt-id/waltid-wallet-backend) _(includes issuer and verifier API backends)_
* **Verifier web portal:** [waltid-verifier-portal](https://github.com/walt-id/waltid-verifier-portal)
* **Issuer web portal:** [waltid-issuer-portal](https://github.com/walt-id/waltid-issuer-portal)

## Web wallet

* **Wallet web app**
  * Web based user interface for managing credentials and DIDs
* **User management**
  * Authorization is currently mocked and not production ready
  * User-context switching and user-specific encapsulated data storage
* **Basic user data management**
  * List dids
  * List credentials
* **Verifiable Credential and Presentation exchange**
  * Support for credential presentation exchange based on OIDC-SIOPv2 spec

## Verifier portal

* **Verifier web portal**
  * Web based user interface for requesting credential presentations through the web wallet
* **Wallet configuration**
  * Possibility to configure list of supported wallets (defaults to walt.id web wallet)
* **Presentation exchange**
  * Support for presentation exchange based on OIDC-SIOPv2 spec

## Issuer portal

* **Issuer web portal**
  * Web based user interface for issuing credentials to the web wallet
* **Wallet configuration**
  * Possibility to configure list of supported wallets (defaults to walt.id web wallet)
* **Verifiable credential issuance**
  * Support for issuing verifiable credentials to the web wallet, based on OIDC-SIOPv2 spec
