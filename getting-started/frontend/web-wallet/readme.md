# Web-wallet

_Web-wallet_ is a solution providing custodial end-user wallet functionality able to satisfy
personal and organizational credential wallet needs. It also offers token (crypto collectibles)
wallet functions (currently limited to asset access) that enable holders to interact with their
assets.

Considering the context, _web-wallet_ offers the following functions:

- [credential functions](#credential-functions) - functions related to verifiable credentials
- [token functions](#token-functions) - functions related to crypto collectibles
- [key functions](#key-functions) - display and manage cryptographic keys
- [DID functions](#did-functions) - display and manage DID methods

## Credential functions

- view - display verifiable credentials
  - JSON format
  - QR-code
- delete - delete a credential from wallet
- receive - accept / reject credentials
  - synchronously - with a request string, either:
    - scanned from a QR code
    - or input explicitly
  - asynchronously - by means of the push-notifications service
- present - scan a credential presentation request

## Token functions

- addresses - display and manage crypto wallets addresses
- tokens - display (and manage) crypto collectibles (NFTs)

## Key functions

- import (JWK or PEM) - import any key (private or public) to wallet
- create - generate a key (private or public)
- view - display key information, such as algorithm, key-id or crypto-provider
- delete - delete a key from wallet
- export (JWK or PEM) - export a wallet key (private or public) using a given format

## DID functions

- view - display the DID document of a DID url
  - JSON format
  - QR-code
- create - create a new DID using the available DID methods