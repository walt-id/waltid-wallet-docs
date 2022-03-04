# Quick start

## Docker compose

The easiest and fastest way to get the wallet **backend**, **frontend**, and **issuer** and **verifier portals** started, is by using our docker-compose configuration, located in the [waltid-wallet-backend](https://github.com/walt-id/waltid-wallet-backend) project, in the subfolder:

`./docker/`.

Simply start the services using:

```
docker-compose up
```

This configuration will publish the following endpoints by default:

* **Web wallet** on _**\[HOSTNAME]:8080**_
  * API: /api/
* **Verifier portal** on _**\[HOSTNAME]:8081**_
  * API: /verifier-api/
* **Issuer portal** on _**\[HOSTNAME]:8082**_
  * API: /issuer-api/

_Note_

**\[HOSTNAME]** is your local computer name. If you use _**localhost**_ instead, some features, particularly with regards to credential exchange, will _**not**_ work correctly.

Visit the `./docker` folder for adjusting the system config in the following files:

* **docker-compose.yaml** - Docker config for launching containers, volumes & networking
* **ingress.conf** - Routing config
* **config/wallet-config.json** - wallet backend configuration
* **config/verifier-config.json** - verifier backend configuration
* **config/issuer-config.json** - issuer backend configuration

Please refer to the configuration section for more details about the available configuration options:

{% content-ref url="../usage/configuration.md" %}
[configuration.md](../usage/configuration.md)
{% endcontent-ref %}

## Build and run

To build and run the services locally, you need _gradle_ and/or _docker_ to build the wallet backend, and _node_ and _yarn_ to build the web applications.

For details of the build process refer to the Build section:

{% content-ref url="../usage/build.md" %}
[build.md](../usage/build.md)
{% endcontent-ref %}

For details of configuration and launch of the services refer to:

{% content-ref url="../usage/configuration.md" %}
[configuration.md](../usage/configuration.md)
{% endcontent-ref %}

{% content-ref url="../usage/run.md" %}
[run.md](../usage/run.md)
{% endcontent-ref %}
