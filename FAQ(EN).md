1. What does SSR stand for?

    SSR is ShadowSocksR, some people call it Shadowsocks-rss which is wrong. RSS
    stands for Really Simple Syndication.

2. Do server side obfuscation parameters need to be the same as the client side
   configuration?

    No. Server side parameters mean differently than client side parameters. Please
    read our documentation carefully. Under multi-user environment, parameters
    values should be defined by server administrator, not by the client user.

3. How to clear bandwidth statistics?

    Shut down the server daemon and delete the `transfer_log.json` file. Restart
    the daemon.

4. Shortcut operations on the tray icon?

    - Left click to edit server list.
    - Middle click / ctrl + left click shows the usage statistics.
    - Right click for menu
    - Shift + right click to bring up settings window.

5. In the tray, what does the colour of the icon mean?

    - Blue: Proxy turned off
    - Green: PAC proxy
    - Cyan: System-wide proxy
    - Red mixed with any of the above: load balancing.

6. What is the quickest way to browse internet with ShadowSocks?

    Depend on your ISP and your location, there are several situations.

    - If no firewall is present, direct connection seems fastest.
    - If the GFW is in place but without any interference on encrypted traffic,
    `origin` and `plain` is the fastest.
    - If GFW with characteristic recognision, but no QoS, set protocol but obfuscation in SSR.
    - If GFW with characteristic recognision + QoS, also set obfuscation.

7. How to setup the most secure configuration?

    Protocol set to `auth_aes128_md5` or `auth_aes128_sha1`, set at least 1 redirect target in server
    parameter. Obfuscation can be anything you like, it does not affect the
    security level.
