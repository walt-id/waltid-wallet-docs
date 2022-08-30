# Quick Start

There are different ways to get started with the Wallet Kit, the Issuer and the Verifier Portal:

* [**Docker Compose**](docker-compose.md): Quick way to launch and try out the latest builds of the services. No build environment required, adaptation of default config may be necessary, depending on your setup.
* [**Local Docker Build**](local-build/#docker-build): Quickly build the individual services using Docker and launch the docker images. No local build environment required, besides docker.
* [**Local Build**](local-build/#local-build): Build and run the services locally on your machine. Requires build environment, including JDK 16, Gradle, NodeJs and Yarn.
* ****[**Dependency (JVM)**](../dependency-jvm.md): The Wallet Kit ("wallet backend") can be used directly as [**JVM-dependency**](../dependency-jvm.md) **** for Maven or Gradle.&#x20;

[**CLI tool**](../cli.md): The Wallet Kit comes with a **** [**command-line interface**](../cli.md) to conveniently configure the issueer DID among other configuration options.

Once you have the Wallet Kit ("wallet backend") up and running, you may want to explore the [**REST APIs**](../rest-apis.md) exposed by the service.

If you just want to try out the Wallet Kit and the Issuer and Verifier Portals, you may directly use our [**public deployments**](../public-deployments.md), which don't require any configuration or build environment.

