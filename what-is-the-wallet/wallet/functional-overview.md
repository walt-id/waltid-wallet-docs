# Wallet 101

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
