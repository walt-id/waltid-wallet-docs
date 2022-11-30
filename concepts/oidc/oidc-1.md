# OIDC for Verifiable Presentations (SIOP)

The verification flow is started from the Verifier web portal. The Verifier requires a certain type of credential to be presented, and lets the user connect to a previously configured, known Wallet. The Wallet, receiving the SIOP authorization request, can check if all required credentials are available, and optionally trigger a credential issuance flow for possibly missing credentials. If all required credentials are available the user selects the credentials to present (if multiple credentials of the same type are available) and the Wallet generates the SIOP response, containing an id token and the verifiable presentation, and redirects back the verifier portal for verification:

![Wallet presentation flow](../../ecosystems-interoperability/siop-vc-presentation.png)
