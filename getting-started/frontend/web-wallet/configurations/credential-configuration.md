# Credential-related configuration

Credential-related configuration specifies the remote location which web-wallet backend should use
to proxy all the credential management, key management, DID management and authentication
requests to. The configuration is stored in the `wallet.conf` file.

e.g. wallet configuration
```hocon
remoteWallet = "https://wallet.walt.id"
```

## Push notifications configuration

A push notifications service can be configured so that the end-user will receive notifications for
both when they receive a credential accept request or a verification request. The configuration is stored
in the `push.conf` file.

e.g. push notifications configuration
```hocon
pushPrivateKey="{server-private-key}"
pushPublicKey="{server-public-key}"
pushSubject="mailto:dev@walt.id"
```