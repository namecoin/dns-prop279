# Tor Prop279 Provider for DNS

`dns-prop279` acts as a bridge between Tor Prop279 clients and DNS servers.  It is designed to be used for Namecoin naming in Tor.  `dns-prop279` is a fork of Miek Gieben's excellent `q` tool.

## Usage

You need [TorNS](https://github.com/meejah/TorNS) in order to use `dns-prop279`.  You also need a DNS server such as [ncdns](https://github.com/namecoin/ncdns).  Your TorNS services configuration might look like this:

~~~
_service_to_command = {
    "bit.onion": ['/path/to/dns-prop279', '-port', '5391', '@127.0.0.1'],
    "bit": ['/path/to/dns-prop279', '-port', '5391', '@127.0.0.1'],
}
~~~

## Security Notes

* `dns-prop279` hasn't been carefully checked for proxy leaks.
* Using `dns-prop279` will make you stand out from other Tor users.
* Stream isolation for streams opened by applications (e.g. Tor Browser) should work fine.  However, stream isolation metadata won't propagate to streams opened by the DNS server.  That means you should only use `dns-prop279` with a DNS server that will not generate outgoing traffic when you query it.  ncdns is probably fine as long as it's using a full-block-receive Namecoin node such as Namecoin Core or libdohj-namecoin in leveldbtxcache mode.  Unbound is not a good idea.
* Nothing in `dns-prop279` prevents the configured DNS server from caching lookups.  If lookups are cached, this could be used to fingerprint users.  ncdns has caching enabled by default.
* DNSSEC support hasn't been tested at all, and is probably totally unsafe right now.  Only use `dns-prop279` when you fully trust the configured DNS server and your network path to it.
* This whole thing is highly experimental!  Please test it and give feedback, but **don't rely on it behaving correctly**.

# Original miekg/exdns README

[![Build Status](https://travis-ci.org/miekg/exdns.svg?branch=master)](https://travis-ci.org/miekg/exdns)
[![BSD 2-clause license](https://img.shields.io/github/license/miekg/exdns.svg?maxAge=2592000)](https://opensource.org/licenses/BSD-2-Clause)

# Examples made with Go DNS

This repository has a bunch of example programs that
are made with the https://github.com/miekg/dns Go package.

Currently they include:

* `as112`: an AS112 black hole server
* `chaos`: show DNS server identity
* `check-soa`: check the SOA record of zones for all nameservers
* `q`: dig-like query tool
* `reflect`: reflection nameserver
