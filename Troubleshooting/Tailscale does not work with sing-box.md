[[nftables]]

## Background

```console
$ ip route
default via 192.168.0.1 dev wlp0s20f3 proto dhcp src 192.168.0.107 metric 600
172.19.0.0/30 dev singbox-tun proto kernel scope link src 172.19.0.1
192.168.0.0/24 dev wlp0s20f3 proto kernel scope link src 192.168.0.107 metric 600
192.168.39.0/24 dev virbr1 proto kernel scope link src 192.168.39.1 linkdown
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 linkdown
```

```console
$ ip route show table 52
100.86.95.104 dev tailscale0
100.100.100.100 dev tailscale0
100.111.208.42 dev tailscale0

$ tailscale status
100.86.54.123   fedora                     yshngg@  linux    -
100.111.208.42  lenovo-xiaoxin-14api-2019  yshngg@  linux    active; relay "nue", tx 18982024 rx 140136660
100.86.95.104   v2231a                     yshngg@  android  offline, last seen 1h ago

$ ip route get 100.111.208.42
100.111.208.42 dev tailscale0 table 52 src 100.86.54.123 uid 1000
    cache
```

## Reference

[Can I use Tailscale alongside other VPNs?](https://tailscale.com/docs/reference/faq/other-vpns#split-tunnels)

[Troubleshoot TCP connection issues between two devices](https://tailscale.com/docs/reference/troubleshooting/network-configuration/tcp-connection-two-devices)

[List of reserved IP addresses](https://en.wikipedia.org/wiki/List_of_reserved_IP_addresses)

[IP routes installed by Tailscale](https://tailscale.com/docs/reference/troubleshooting/network-configuration/tailscale-ip-routes)

[Netfilter hooks](https://wiki.nftables.org/wiki-nftables/index.php/Netfilter_hooks)

[Ruleset debug/tracing](https://wiki.nftables.org/wiki-nftables/index.php/Ruleset_debug/tracing)
