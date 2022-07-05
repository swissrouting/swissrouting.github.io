# Core Lightning Tips

Tips and tricks to configure Core Lightning (CLN) for optimal performance.

## Config File

See [lightningd docs](https://lightning.readthedocs.io/lightningd-config.5.html) for more details.

### Config File Location

On the RaspiBlitz you can find the CLN config file under the `bitcoin` user's home directory.

    /home/bitcoin/.lightning/config

## Hybrid Mode

### What Is Hybrid Mode?

"Hybrid" mode is a terminology popularized by LND but the same concept can apply to CLN.

This simply means that outgoing connections to Tor nodes are routed through the Tor proxy, while outgoing connections to clearnet nodes are routed directly over the normal "clearnet" network.

### How To Enable/Disable

Hybrid mode in CLN is controlled using the option `always-use-proxy`.

To enable hybrid mode, add this line to your config file:

    # Note: this is also the default setting if unspecified
    always-use-proxy=false
    
To disable hybrid mode (and route all traffic through Tor proxy), change it to:

    always-use-proxy=true

### Configuration Sample

My config file looks something like:

    # TOR
    addr=statictor:127.0.0.1:9051/torport=9735
    proxy=127.0.0.1:9050
    always-use-proxy=false

    # CLEARNET
    bind-addr=0.0.0.0:<ClearnetPort>
    announce-addr=<ClearnetIp>:<ClearnetPort>
