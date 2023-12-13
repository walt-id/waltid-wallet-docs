# Quick Start

{% hint style="danger" %}
### Discontinuation Notice

**Important:** Please be informed that, beginning from December 2023, the Wallet Kit will no longer receive new features. Furthermore, the Wallet Kit is planned for discontinuation by the end of Q3 2024.\
\
However, all functionalities currently offered by the Wallet Kit will be integrated into our new libraries, APIs, and apps under [The Community Stack](https://walt.id/blog/p/community-stack). This is aimed at providing a more modular, flexible, and efficient solution for your needs.\
\
For any clarification or queries, feel free to [contact us](https://walt.id/discord) as we aim to make this transition as smooth as possible.
{% endhint %}

There are different ways to get started with the Wallet Kit, either you use the backend service provided by the Wallet Kit via an API and build your own frontend, or you run the Wallet Kit (backend service) and our pre-build frontend applications for the different parties (holder, verifier, issuer).&#x20;

* [**Docker Compose**](docker-compose.md): Quick way to launch and try out the latest builds of our backend API and the frontend applications. No build environment required, adaptation of default config may be necessary, depending on your setup.
* [**Local Docker Build**](local-build/#docker-build): Quickly build the individual services (backend & frontend) using Docker and launch the docker images. No local build environment required, besides docker.
* [**Local Build**](local-build/#local-build): Build and run the services (backend & frontend) locally on your machine. Requires build environment, including JDK 16, Gradle, NodeJs and Yarn.
* [**Dependency (JVM)**](../dependency-jvm.md): The Wallet Kit ("wallet backend") can be used directly as [**JVM-dependency**](../dependency-jvm.md) for Maven or Gradle.&#x20;

[**CLI tool**](../cli.md): The Wallet Kit comes with a [**command-line interface**](../cli.md) to conveniently configure the issueer DID among other configuration options.

[**REST API**](../rest-apis/) - Use the Wallet Kit API.

[**Frontend Applications**](../frontend/) - Offer a full solution by connecting our pre-build frontends to the Wallet Kit backend.

If you just want to try out the Wallet Kit (backend) and the Issuer and Verifier Portals (frontends), you may directly use our [**public deployments**](../public-deployments.md), which don't require any configuration or build environment.

