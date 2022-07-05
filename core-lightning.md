# Core Lightning Tips

Tips and tricks to configure Core Lightning (CLN) for optimal performance.

## Where To Find The Config File

    ~/.lightning/config

## How To Enable Hybrid Mode

CLN actually uses so-called "hybrid" mode by default. This means that outgoing connections to Tor nodes are routed through the Tor proxy, while outgoing connections to clearnet nodes are routed directly through the default gateway.

The terminology has been popularized by LND but the same concept can apply to CLN.

To enable hybrid mode, add this line to your config file:

    always-use-proxy=false
    
To disable hybrid mode (and route all traffic through Tor proxy), change it to:

    always-use-proxy=true
    
My config file looks something like:

    # TOR
    addr=statictor:127.0.0.1:9051/torport=9735
    proxy=127.0.0.1:9050
    always-use-proxy=false

    # CLEARNET
    bind-addr=0.0.0.0:<ClearnetPort>
    announce-addr=<ClearnetIp>:<ClearnetPort>
