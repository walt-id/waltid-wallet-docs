# Introduction

This documentation will help you understand how the Wallet works and how you can use it. However, it presumes a certain level of knowledge about Self-Sovereign Identity (SSI) so

* if you are already familiar with SSI, you can jump to the [introduction of the Wallet](what-is-the-wallet/wallet/).&#x20;
* if you are new to SSI, please continue with our introduction to Self-Sovereign Identity.

The walt.id SSI web wallet, is an implementation of a custodial wallet, for end users to manage their SSI data, such as keys, decentralized identifiers (DIDs) and verifiable credentials.

The wallet provides storage and management of SSI data, as well as, an integration of credential exchange protocols, such as OIDC and SIOP, for credential issuance and presentation.

We provide a wallet backend service, implementing the business logic and RESTful APIs: [**waltid-wallet-backend**](https://github.com/walt-id/waltid-wallet-backend), as well as a web wallet frontend, implementing the user experience: [**waltid-web-wallet**](https://github.com/walt-id/waltid-web-wallet)

Additionally, we provide reference implementations of verifier and issuer web portals, showcasing the credential issuance and credential presentation flows: [**waltid-issuer-portal**](https://github.com/walt-id/waltid-issuer-portal) and [**waltid-verifier-portal**](https://github.com/walt-id/waltid-verifier-portal), where the backend business logic and APIs of the verifier and issuer portals are integrated in the _wallet-backend_ service.

## Project sources

* **Web wallet frontend:** [waltid-web-wallet](https://github.com/walt-id/waltid-web-wallet)
* **Wallet API backend** [waltid-wallet-backend](https://github.com/walt-id/waltid-wallet-backend) _(includes issuer and verifier API backends)_
* **Verifier web portal:** [waltid-verifier-portal](https://github.com/walt-id/waltid-verifier-portal)
* **Issuer web portal:** [waltid-issuer-portal](https://github.com/walt-id/waltid-issuer-portal)
