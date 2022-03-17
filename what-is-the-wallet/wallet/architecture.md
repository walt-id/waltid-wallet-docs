# Architecture

It is is a custodial wallet that enables end users to manage their keys, Decentralized Identifiers (DIDs) and Verifiable Credentials/Presentations (VCs, VPs).

The Wallet provides storage and management of SSI data, as well as, an integration of credential exchange protocols (e.g. OIDC, SIOP) for credential issuance and presentation exchange.

We provide

* Wallet backend service, implementing the business logic and RESTful APIs: [waltid-wallet-backend](https://github.com/walt-id/waltid-wallet-backend)
* Web Wallet frontend, implementing the user experience: [waltid-web-wallet](https://github.com/walt-id/waltid-web-wallet)

Additionally, we provide reference implementations of verifier and issuer web portals, showcasing the credential issuance and credential presentation flows: [waltid-issuer-portal](https://github.com/walt-id/waltid-issuer-portal) and [waltid-verifier-portal](https://github.com/walt-id/waltid-verifier-portal), where the backend business logic and APIs of the verifier and issuer portals are integrated in the _wallet-backend_ service.

##
