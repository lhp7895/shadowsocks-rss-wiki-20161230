# Python client setup guide

Basic setup
--
If not mentioned, the following steps are run by the `root` user.

CentOS:

`yum install m2crypto git`

Ubuntu/Debian:
`apt-get install m2crypto git`

Windows:
[Server-Setup-on-Windows](https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup-on-Windows)

Obtain source code
--
`git clone -b manyuser https://github.com/breakwa11/shadowsocks.git`

Enter subdirectory `shadowsocks/shadowsocks`

## Running via command line
```
python local.py -s <server_ip> \
                -p <port> \
                -k <keyphrase> \
                -m <encryption> \
                -o <obfus> \
                -l <local_port>
```

Replace `<variable>` with appropriate values.

If require daemonization, append `-d start` on the above command. To stop or restart, execute
`python local.py -d stop # or restart`

Check logs:
```
tail -f /var/log/shadowsocks.log
```

`-h` shows the documentation.

## Running via configuration file

Create a configuration file at `/etc/shadowsocks.json`

Write the configuration:
```javascript
{
    "server":"0.0.0.0",
    "server_ipv6": "::",
    "server_port": <port>,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"<password>",
    "timeout":300,
    "method":"<encryption>",
    "obfs":"<obfs>",
    "fast_open": false,
    "workers": 1
}
```

Replace `<variables>` with appropriate values.

Then execute the following commands:

```
python local.py -c /etc/shadowsocks.json
```

You may combine with `-d start/restart/stop` options to initialize/restart/stop
the daemon.

Proxy setup
--

Default address: `127.0.0.1`
Default port: 1080

Note: Python client only supports SOCKS proxy.

Install on Windows server: [https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup-on-Windows](https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup-on-Windows)
