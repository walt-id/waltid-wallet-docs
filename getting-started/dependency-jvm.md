# Dependency (JVM)

The wallet can also be used as direct dependency for JVM-based applications. In this case an existing application can easily be transformed into an SSI wallet backend.

The following illustrates how the wallet-backend can be used via Gradle:

Repositories

`maven("https://maven.walt.id/repository/waltid/")`&#x20;
`maven("https://maven.walt.id/repository/waltid-ssi-kit/")`&#x20;
`maven("https://repo.danubetech.com/repository/maven-public/")`&#x20;
`maven("https://jitpack.io/")`&#x20;

Dependency

`implementation("id.walt:waltid-walletkit:<version>")`

The code-base as well as more detailed instructions can be found at GitHub  [https://github.com/walt-id/waltid-walletkit](https://github.com/walt-id/waltid-walletkit)
