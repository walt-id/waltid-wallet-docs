# Wallet 101

## What is the Wallet?

The Wallet is **everything you need to launch identity wallets**.

What you need to know about the Wallet:

* It is **open source (Apache 2)**. You can use the code for free and without limitations. &#x20;
* It is an **out-of-the-box solution** that you can simply re-use or even white-label such as for building pilot projects quickly.
* It is **customisable** in a sense that you can individualise the app based on your requirements: You can rebrand the app, build your own UI and add new features. ****&#x20;
* It is **composable** in a sense that you can plug your existing (d)apps into the wallet backend in order to supercharge your (d)apps with SSI capabilities.&#x20;
* It **abstracts complexity** such as low-level functionality related to key handling, data storage, signing and interactions with third party systems.
* It is **built on open standards** to ensure interoperability and prevent lock-in effects.
* It is **flexible** in a sense that you can deploy and run the Wallet on-premise, in your (multi) cloud environment or directly integrate our libraries.&#x20;
* It enables you to **use different identity ecosystems** like Europe’s emerging identity ecosystem ([EBSI, ESSIF](https://ec.europa.eu/digital-building-blocks/wikis/display/ebsi)) in anticipation of a multi-ecosystem future.

## How does the Wallet work?

You can think about the Wallet as **a secure data hub for organizations and individuals** that enables users to act as a Holder, i.e. to store, manage and share keys, identity data (e.g. Verifiable Credentials) and other secrets.&#x20;

Practically speaking, the Wallet is a tool that allows users to **collect and present identity data** to access services (e.g. sign up, login, check-out) or procure products in a seamless and privacy preserving fashion. To enable the exchange of data between parties, the Wallet integrates credential exchange protocols (e.g. OIDC, SIOP) for credential issuance and presentation exchange.

Since there are different types of wallets, it is important to mention that our Wallet is a so-called **“custodial” wallet**, i.e. keys and data are ultimately stored by the provider of the Wallet. While this means less control for end-users, it also means a better user experience (e.g. no need to handle private keys or seed phrases) and less risk (e.g. loss of data). Also, end-users enjoy full data portability, i.e. they can switch wallets (and take their keys and data with them) at any time without having to fear lock-in effects.

## How you can use the Wallet

There are three ways you can use the Wallet:

![](<../../.gitbook/assets/Screenshot 2022-03-17 at 11.54.22.png>)

_Note: The easiest and fastest way to launch a wallet is to take our web-app and rebrand it. Also, this web-based approach has a number of advantages. For example, it works on every platform/device, users do not require multiple devices and must not scan QR Codes. More importantly a web-based flow is very user friendly and leverages learned behaviour from federated identity providers (“login with Google”)._

## Project Sources

* **Wallet backend service**, implementing the business logic and RESTful APIs: [waltid-wallet-backend](https://github.com/walt-id/waltid-wallet-backend) (including Issuer and Verifier API backends)
* **Web Wallet frontend**, implementing the user experience: [waltid-web-wallet](https://github.com/walt-id/waltid-web-wallet)
* Additionally, we provide reference implementations of **Issuer and Verifier Portals**, showcasing the credential issuance and credential presentation flows: [waltid-issuer-portal](https://github.com/walt-id/waltid-issuer-portal) and [waltid-verifier-portal](https://github.com/walt-id/waltid-verifier-portal), where the backend business logic and APIs of the verifier and issuer portals are integrated in the _wallet-backend_ service.&#x20;

