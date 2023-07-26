# Storage configuration

Storage configuration describes which database should be used for data persistence and the
connection string required to connect to the database. This information is organized in 2 separate
configuration files:
1. `db.conf` - [database configuration](#database-configuration)
2. `db.<database>.conf` - [datasource configuration](#datasource-configuration)

## Database configuration

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

## Datasource configuration

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