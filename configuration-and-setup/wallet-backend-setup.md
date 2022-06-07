# Wallet backend setup

To setup the wallet backend a few things may need to be considered and some configuration may be required, depending on your situation.

## Data root

Configuration and data are kept in sub folders of the **data root** *(by default the current working directory)*:

* `config/`
* `data/`

To override the **data root**, one can set the **environment variable**: 

`WALTID_DATA_ROOT`

## Wallet backend configuration

The configuration of the wallet backend can be adapted, by modifying the file 

`config/wallet-config.json`

### External URLs

Configure the URLs via which the wallet UI and API will be reachable from an external source, e.g. a web browser, or a verifier service requesting a presentation exchange:

```
{
  "walletUiUrl": "http://wallet.waltid.org",
  "walletApiUrl": "http://wallet.waltid.org/api",
[...]
}
```

### OIDC Issuers

To enable OIDC credential issuance from within the wallet, you need to configure where the wallet finds the issuer APIs and possibly how to authenticate with the OIDC service.

For each known issuer configure a unique **id**, a **description** and the **OIDC base URL**. From this base URL, the wallet will try to resolve the OIDC discovery document at `<base-url>/.well-known/openid-configuration`.

```
{
  [...]
  "issuers": {
    "walt.id": {
      "id": "walt.id",
      "url": "http://issuer.waltid.org/issuer-api/oidc",
      "description": "walt.id Issuer Portal"
    },
    [...]
}
```
#### Secrets

If you need to authenticate with the OIDC issuer service, you can configure the **client_id** and **client_secret** in two ways:

1) **In wallet-config.json**

```
{
  [...]
  "issuers": {
    "walt.id": {
      "id": "walt.id",
      [...]
      "client_id": "FOO",
      "client_secret": "BAR"
    },
    [...]
}
```

2) **In a separate file**

To keep the secrets separated from the main config, you can keep them in a separate file, relative to the **data root**:

`secrets/issuers.json`

```
{
  "secrets": {
    "walt.id": {
      "client_id": "FOO",
      "client_secret": "BAR"
    }
  }
}
```

You can make use of this separate secrets file, e.g. in a **Kubernetes** or **Docker Swarm** deployment, to keep the passwords in a safe secret object.
Also, it enables you to separate the secrets from the default configuration, which you may want to check in to version control.

### Configuration example

Here's a complete example for the **wallet-config.json**:

```
{
  "walletUiUrl": "https://wallet.waltid.org",
  "walletApiUrl": "https://wallet.waltid.org/api",
  "issuers": {
    "walt.id": {
      "id": "walt.id",
      "url": "https://issuer.waltid.org/issuer-api/oidc",
      "description": "walt.id Issuer Portal"
    }
  }
}
```

## Wallet backend data storage

User data (dids, keys, credentials) are currently stored in subfolders for the **user id**, like so:

`<data_root>/data/<user@email.com>`

It is planned to allow users to define their own storage preferences, in the future.

## Binding address and port

By default, the wallet backend exposes its API endpoints bound to **localhost**, port **8080**.

To override the default bindings, set the following **environment variables**:

`WALTID_WALLET_BACKEND_BIND_ADDRESS`

`WALTID_WALLET_BACKEND_PORT`

### Command arguments
To set binding address and port, you can also use the command arguments of the **run** command like so:

_To set the bind address to "192.168.0.1" and the port to 8081_:
```
waltid-walletkit run -b "192.168.0.1" -p 8081
```

_To bind to all interfaces (on the default port)_:
```
waltid-walletkit run --bind-all
```
