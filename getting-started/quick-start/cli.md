# CLI - command line interface

The wallet backend provides a simple command line interface, to run and/or configure the backend, including the issuer and verifier backends.

## Run

To run the backend service type

```
waltid-wallet-backend run
```

{% hint style="info" %}
To get info about available options of the **run** command, use:

`waltid-wallet-backend run --help`
{% endhint %}

## Config

For configuring DIDs and keys (especially for the issuer or verifier backends), the wallet backend integrates some commands from the **walt.id SSI Kit** for key and did management.

The config command lets you define the context in which you want to execute the command, by specifying the arguments _--as-issuer_, _--as-verifier_ or _--as-user \[userID]_ or their shortcuts _-i_, _-v_, _-u \[userID]_, before the respective subcommand.

_E.g._

```
waltid-wallet-backend config --as-issuer did list
```

This command lists the DIDs available in the context of the _issuer backend_.

{% hint style="info" %}
To get info about available options of the **config** command, use:

`waltid-wallet-backend config --help`
{% endhint %}

For more details about the integrated commands, you may want to refer to the documentation of the **SSI Kit**, which you find here:

{% content-ref url="https://app.gitbook.com/o/ZPIzdSlXqm9n9ywE2dcK/s/1k3zreXT6Nz41D1g1C6K/" %}
[SSIKit](https://app.gitbook.com/o/ZPIzdSlXqm9n9ywE2dcK/s/1k3zreXT6Nz41D1g1C6K/)
{% endcontent-ref %}
