# REST APIs

### Installation & Running the Project

Make sure you have Docker or a JDK 16 build environment including Gradle installed on your machine

{% tabs %}
{% tab title="Docker Build" %}
1. Cloning the project

```
git clone https://github.com/walt-id/waltid-walletkit.git
```

2\. Change directory

```
cd waltid-walletkit
```

3\. Build docker container

```
docker build --rm -t waltid/walletkit .
```

4\. Run the project

```
docker run -p 8080:8080 -e WALTID_DATA_ROOT=/data -v $PWD:/data waltid/walletkit run
```
{% endtab %}

{% tab title="Local Build" %}
1. Cloning the project

```
git clone https://github.com/walt-id/waltid-walletkit.git
```

2\. Change directory

```
cd waltid-walletkit
```

3\. Build the project

```
./gradlew build install
```

4\. Create an alias

```
alias walletkit="build/install/waltid-walletkit/bin/waltid-walletkit"
```

Note: The alias will only exist during the current terminal session and must be set again in any sessions their after

5\. Run the project (with alias)

```
walletkit run
```

6\. Run the project (without alias)

```
build/install/waltid-walletkit/bin/waltid-walletkit run
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
To get info about available options of the **run** command, use:

`waltid-walletkit run --help`
{% endhint %}

More options how to build the project can be found [here](../quick-start/local-build/).

## Binding address and port

Refer to the [wallet backend configuration section](../../configuration-and-setup/wallet-backend-setup.md#binding-address-and-port), to find out details about binding address and port on which the REST APIs are exposed.

## API services

* **Wallet API** is available under the context path `/api/`
* **Verifier portal API** is available under the context path `/verifier-api/`
* **Issuer portal API** is available under the context path `/issuer-api/`

## Swagger API documentation

A **swagger documentation** _(including the issuer and verifier API documentation)_ is available under `/api/swagger`

## Frontend

If you want to start using the API right away without setting up a custom frontend application, you can use one of our [prebuild](./#undefined) ones to get started.

## Publicly deployed API documentation

You can find the publicly deployed API documentations for our [stable and rolling deployments](../public-deployments.md):

* **Rolling deployment:** [https://wallet.walt-test.cloud/api/swagger](https://wallet.walt-test.cloud/api/swagger)
* **Stable deployment:** [https://wallet.walt.id/api/swagger](https://wallet.walt.id/api/swagger)

### Get started with the following:

[Issuer Configuration](issuer-configuration.md) - How to configure your issuer with multi tendency support

[Credential Templates](credential-templates.md) - Define schemas on which you can issue Verifiable Credentials

[Credential Issuance](credential-issuance-api.md) -  How to issue Verifiable Credentials based on previous defined templates

[Credential Verification](credential-verification.md) - How to verify Verifiable Credentials
