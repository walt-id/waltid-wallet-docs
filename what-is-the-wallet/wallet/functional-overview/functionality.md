# Functionality

The Wallet is **a secure data hub for organizations and individuals** and enables them to act as "Holders": They can store, manage and share keys, identity data (e.g. Verifiable Credentials) and other secrets.

Practically speaking, the Wallet is a tool that allows anyone to **collect and present identity data** in order to access services (e.g. sign up, login, check-out) or procure products in a seamless and privacy preserving fashion.

Since there are different types of wallets, it is important to mention that our Wallet is a so-called **custodial wallet**, which means that keys and data are ultimately stored by the wallet provider. While this means less control for end-users, it also means a better user experience (e.g. no need to handle private keys or seed phrases) and less risk (e.g. loss of data). Also, end-users enjoy full data portability, which means that they can switch wallets (and take their keys and data with them) at any time without having to fear lock-in effects.

## Functional overview

Here's an overview of the functionality provided by the wallet:

* **Wallet web app**
  * Web based user interface for managing credentials and DIDs
* **User context separation**
  * Separation of user contexts in the data stores (key store, credential store, DID store)
* **User data management**
  * DIDs
  * Credentials
* **Ecosystems integration**
  * EBSI/ESSIF 
    * DID registration
  * did:web 
    * DID creation
    * DID web registry
  * did:key
    * DID creation
* **Verifiable Credential and Presentation exchange**
  * Support for credential and presentation exchange based on the OIDC and SIOP specs
    * OIDC4CI - OIDC for credential issuance
    * OIDC4VP - OIDC for verifiable presentations (SIOP)
    * Credential issuance via SIOP protocol (custom protocol)
    