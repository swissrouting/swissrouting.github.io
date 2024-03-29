# Nostr Tips
{: .no_toc }

Tips and tricks for getting the most out of Nostr.

## Table Of Contents
{: .no_toc }

1. TOC
{:toc}

## What is Nostr?

[Nostr](https://github.com/nostr-protocol/nostr) is the simplest open protocol that is able to create a censorship-resistant global "social" network once and for all.

It doesn't rely on any trusted central server, hence it is resilient; it is based on cryptographic keys and signatures, so it is tamperproof; it does not rely on P2P techniques, therefore it works.

## Getting Started

See [this guide](https://github.com/vishalxl/nostr_console/discussions/31) for suggestions about how to get started using Nostr.

## nostream

### Setting Up

One of the most popular relay implementations is [nostream](https://github.com/Cameri/nostream). You can install it on your system by following the [excellent guide by André Neves](https://andreneves.xyz/p/set-up-a-nostr-relay-server-in-under).

### Installing as a Service

If you want your relay to restart automatically on failure, and to start up again after every reboot, you can install it as a systemd service.

First open your favorite editor and save the following contents into a file under `/etc/systemd/system`. For example:

    nano /etc/systemd/system/nostream.service

{% gist 76175d24b3b233bf331b59d7643f0d3c %}

Then you can enable it to launch at boot with:

    systemctl enable nostream

Start it with:

    systemctl start nostream

And view the logs with:

    journalctl -u nostream

### Mirroring

Here's how to mirror events from one Nostr relay to another:

{% gist 78c8bdade7ac5e8e7961b4b4571b7bdd %}
