# Quick Start

There are different ways to get started with the Wallet, the Issuer and the Verifier Portal:

* [**Docker Compose**](docker-compose.md): Quick way to launch and try out the latest builds of the services. No build environment required, adaptation of default config may be necessary, depending on your setup.
* [**Local Docker Build**](local-build/#docker-build): Quickly build the individual services using Docker and launch the docker images. No local build environment required, besides docker.
* [**Local Build**](local-build/#local-build): Build and run the services locally on your machine. Requires build environment, including JDK 16, Gradle, NodeJs and Yarn.
* ****[**Dependency (JVM)**](dependency-jvm.md): The wallet-backend can be used directly as [**JVM-dependency**](dependency-jvm.md) **** for Maven or Gradle.&#x20;

[**CLI tool**](../cli.md):  The wallet-backend comes with a **** [**command-line interface**](../cli.md) to conveniently configure the issueer DID among other configuration options.

Once you have the wallet backend up and running, you may want to explore the [**REST APIs**](../rest-apis.md) exposed by the service.

If you just want to try out the wallet and the issuer and verifier portals, you may directly use our [**public deployments**](../public-deployments.md), which don't require any configuration or build environment.

