# Architecture

The Wallet Kit (or "wallet backend") as well as the Issuer and Verifier Portals (or "Issuer and Verifier backends") are built as abstraction layers over the SSI Kit. They provide **user data separation** (user contexts of the underlying data stores) and **high-level APIs** for the interaction with the **web frontends** and the **credential exchange protocols** such as OIDC and SIOP.

Moreover, the wallet backend can be seen as an abstraction over the "Custodian" component of the SSI Kit, whereas the Issuer and Verifier backends build on top of the "Signatory" and "Auditor" components, respectively.

![Wallet architecture](architecture.png)
