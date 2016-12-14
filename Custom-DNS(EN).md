# Custom DNS

Custom DNS is configured at the server side.

It allows the clients who requested remote DNS resolution on the server side to
query the domain names against whichever DNS server this file specifies.

## Configuration 1
At the configuration folder (where `config.json` resides), put a new file
called `DNS.conf` with the following content:

```
8.8.8.8 53
8.8.4.4 53
```

1 DNS server per line, port number is optional. Server software versions less
than 2.9.4 do not support this feature.

## Configuration 2
Use the System default `resolv.conf`.

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

Does not support non-53 ports by design.

If both are used, Configuration 1 will be eventually used.
