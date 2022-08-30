# CLI | Command Line Interface

The wallet backend provides a simple command line interface, to run and/or configure the backend, including the issuer and verifier backends.

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

4\. Create an alias

```
alias waltid-walletkit="docker run -p 8080:8080 -e WALTID_DATA_ROOT=/data -v $PWD:/data waltid/walletkit"
```

4\. Run the project (with alias)

```
waltid-walletkit run
```

6\. Run the project (without alias)

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
alias waltid-walletkit="build/install/waltid-walletkit/bin/waltid-walletkit"
```

Note: The alias will only exist during the current terminal session and must be set again in any sessions their after

5\. Run the project (with alias)

```
waltid-walletkit run
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

## Configuration

For configuring keys and DIDs (especially for the Issuer or Verifier backends), the wallet backend integrates some commands from the SSI Kit for key and DID management.

The config command lets you define the context in which you want to execute the command, by specifying the arguments _--as-issuer_, _--as-verifier_ or _--as-user \[userID]_ or their shortcuts _-i_, _-v_, _-u \[userID]_, before the respective subcommand.

_E.g._

```
waltid-walletkit config --as-issuer did list
```

This command lists the DIDs available in the context of the _issuer backend_.

{% hint style="info" %}
To get info about available options of the **config** command, use:

`waltid-walletkit config --help`
{% endhint %}

For more details about the integrated commands, you may want to refer to the documentation of the SSI Kit, which you find here:

{% content-ref url="https://app.gitbook.com/o/ZPIzdSlXqm9n9ywE2dcK/s/1k3zreXT6Nz41D1g1C6K/" %}
[SSI Kit](https://app.gitbook.com/o/ZPIzdSlXqm9n9ywE2dcK/s/1k3zreXT6Nz41D1g1C6K/)
{% endcontent-ref %}
