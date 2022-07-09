# Core Lightning Tips
{: .no_toc }

Tips and tricks to configure Core Lightning (CLN) for optimal performance.

## Table Of Contents
{: .no_toc }

1. TOC
{:toc}

## Config File

See [lightningd docs](https://lightning.readthedocs.io/lightningd-config.5.html) for more details.

### Config File Location

On the RaspiBlitz you can find the CLN config file under the `bitcoin` user's home directory.

    /home/bitcoin/.lightning/config
    
## Uptime Monitoring With Amboss

Amboss provides [a health check service](https://docs.amboss.space/api/monitoring/health-checks) for Lightning nodes. This can be used to monitor the uptime of your node and even display the information publicly if desired.

### Cron Configuration

You can use [this shell script for CLN](https://gist.github.com/swissrouting/111d4a615d670ddf8d11eaa8a60eacca) or [this shell script for LND](https://gist.github.com/C-Otto/cd5d7b0e67fc2e3e212cf13a558b101f) on your node to send a properly formed health check ping to Amboss.

{% gist 111d4a615d670ddf8d11eaa8a60eacca %}

1. Save shell script (e.g. `/home/bitcoin/amboss-ping.sh`)
1. Run `crontab -e` to edit your cron configuration
1. Add a line like this to run the job every minute:

```
* * * * * /home/bitcoin/amboss-ping.sh
```

Note: cron scripts do not run with the same environment as interactive shells. This means you might encounter problems when running this script as a cron job. If necessary you can try tweaking the line as follows to instruct bash to use an interactive (login) shell with full user customizations active:

```
* * * * * bash -l /home/bitcoin/amboss-ping.sh
```

### Amboss Configuration

No special configuration is needed on Amboss to enable this functionality.

You can view health checks for your node at: `https://amboss.space/node/NODE_ID/health`

You can configure the health check interval and whether the results are publicly visible at: [https://amboss.space/owner?page=monitoring](https://amboss.space/owner?page=monitoring)

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
