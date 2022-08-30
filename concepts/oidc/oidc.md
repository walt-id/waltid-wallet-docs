# OIDC for Credential Issuance

The issuance flow is triggered from the Wallet. The user chooses from a list of issuer portals known to the wallet backend, and triggers the issuance of any credential type the issuers support. Optionally, the Issuer can require a Verifiable Presentation of certain types of credentials, to be sent with the initial authorization request. If the authorization is successful, the issuer portal provides an authorization code and access token, for the wallet to retrieve the issued credentials:

![](../../ecosystems-interoperability/oidc-credential-issuance.png)
