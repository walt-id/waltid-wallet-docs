# Quick Start

There are different ways to get started with the Wallet Kit, either you use the backend service provided by the Wallet Kit via an API and build your own frontend, or you run the Wallet Kit (backend service) and our pre-build frontend applications for the different parties (holder, verifier, issuer).&#x20;

* [**Docker Compose**](docker-compose.md): Quick way to launch and try out the latest builds of our backend API and the frontend applications. No build environment required, adaptation of default config may be necessary, depending on your setup.
* [**Local Docker Build**](local-build/#docker-build): Quickly build the individual services (backend & frontend) using Docker and launch the docker images. No local build environment required, besides docker.
* [**Local Build**](local-build/#local-build): Build and run the services (backend & frontend) locally on your machine. Requires build environment, including JDK 16, Gradle, NodeJs and Yarn.
* ****[**Dependency (JVM)**](../dependency-jvm.md): The Wallet Kit ("wallet backend") can be used directly as [**JVM-dependency**](../dependency-jvm.md) **** for Maven or Gradle.&#x20;

[**CLI tool**](../cli.md): The Wallet Kit comes with a **** [**command-line interface**](../cli.md) to conveniently configure the issueer DID among other configuration options.

[**REST API**](../rest-apis.md) **** - **** Use the Wallet Kit backend and connect your own frontend.

If you just want to try out the Wallet Kit (backend) and the Issuer and Verifier Portals (frontends), you may directly use our [**public deployments**](../public-deployments.md), which don't require any configuration or build environment.

