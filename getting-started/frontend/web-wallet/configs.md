# Backend configurations

_Web-wallet_ backend is configured with a set of files describing the configuration using the
_Hocon_ format (Human-Optimized Config Object Notation). Things that can be configured relate to:
- [host](#host-configuration)
- [credential dependencies](#credential-related-configuration)
  - [push notifications](#push-notifications-configuration)
- [token dependencies](#token-related-configuration)
- [storage](#storage-configuration)

## Host configuration

Host configuration is specified by the `web.conf` file. It describes the host address and port number.

e.g. host configuration
```
webHost = "0.0.0.0"
webPort = 4545
```

## Credential-related configuration

Credential-related configuration specifies the remote location which web-wallet backend should use
to proxy all the credential management, key management, DID management and authentication
requests to. The configuration is stored in the `wallet.conf` file.

e.g. wallet configuration
```hocon
remoteWallet = "https://wallet.walt.id"
```

### Push notifications configuration

A push notifications service can be configured so that the end-user will receive notifications for
both when they receive a credential accept request or a verification request. The configuration is stored
in the `push.conf` file.

e.g. push notifications configuration
```hocon
pushPrivateKey="{server-private-key}"
pushPublicKey="{server-public-key}"
pushSubject="mailto:dev@walt.id"
```


## Token-related configuration

Token-related configurations hold data about tokens...... . Currently there are 2 configurations
available:
- [chain explorer configuration](#chain-explorer-configuration)
- [marketplace configuration](#marketplace-configuration)

### Chain explorer configuration

Chain explorer configuration holds the mapping of chains to their respective explorer location.
It is stored in the `chainexplorer.conf` file.

e.g. chain explorer configuration
```hocon
explorers = [
    {chain = "ethereum", url = "https://etherscan.io/address/%s"},
    {chain = "polygon", url = "https://polygonscan.com/address/%s"},
    {chain = "mumbai", url = "https://mumbai.polygonscan.com/address/%s"},
    {chain = "tezos", url = "https://tzkt.io/%s/operations"},
    {chain = "ghostnet", url = "https://ghostnet.tzkt.io/%s/operations"},
]
```

### Marketplace configuration

Marketplace configuration holds the mapping of chains to their respective marketplace location.
It is stored in the `marketplace.conf` file.

e.g. marketplace configuration
```hocon
marketplaces = [
    {chain = "ethereum", name = "OpenSea", url = "https://opensea.io/assets/ethereum/%s/%s"},
    {chain = "polygon", name = "OpenSea", url = "https://opensea.io/assets/matic/%s/%s"},
    {chain = "tezos", name = "Rarible", url = "https://rarible.com/token/tezos/%s/%s"},
    {chain = "flow", name = "FlowVerse", url = "https://nft.flowverse.co/marketplace/%s/%s"},
    {chain = "unique", name = "Unique", url = "https://unqnft.io/market/%s/%s"},
]
```

## Storage configuration

Storage configuration describes which database should be used for data persistence and the
connection string required to connect to the database. This information is organized in 2 separate
configuration files:
1. `db.conf` - [database configuration](#database-configuration)
2. `db.<database>.conf` - [datasource configuration](#datasource-configuration)

### Database configuration

Database configuration holds the datasource configuration filename (with no extension).
The content of this configuration file will then be loaded and used as the connection string
for the database. The filename should have the following format: `db.<database>`
The database configuration is stored in the `db.conf` file.

e.g. sqlite database configuration
```hocon
database = "db.sqlite"
```

e.g. postgres database configuration
```hocon
database = "db.postgres"
```

### Datasource configuration

Datasource configuration holds the connection string used to connect to the database.

e.g. sqlite datasource configuration - `db.sqlite.conf`
```hocon
hikariDataSource {
    jdbcUrl = "jdbc:sqlite:data.db"
    driverClassName = "org.sqlite.JDBC"
    username = ""
    password = ""
    transactionIsolation = "TRANSACTION_SERIALIZABLE"
    maximumPoolSize = 5
    autoCommit = false
    dataSource {
        journalMode = "WAL"
        fullColumnNames = false
    }
}
```

e.g. postgres datasource configuration - `db.postgres.conf`
```hocon
hikariDataSource {
    jdbcUrl = "jdbc:postgresql://127.0.0.1:5432/postgres"
    driverClassName = "org.postgresql.Driver"
    username = "postgres"
    password = "postgres"
    transactionIsolation = "TRANSACTION_SERIALIZABLE"
    maximumPoolSize = 5
    minimumIdle: 0
    autoCommit = false
    dataSource {
        journalMode = WAL
        fullColumnNames = false
    }
}
```