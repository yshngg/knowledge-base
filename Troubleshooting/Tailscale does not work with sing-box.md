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

## Analyze

### Routing Policy

```console
$ ip rule
0:	from all lookup local
1:	from all lookup 1337276116
5210:	from all fwmark 0x80000/0xff0000 lookup main
5230:	from all fwmark 0x80000/0xff0000 lookup default
5250:	from all fwmark 0x80000/0xff0000 unreachable
5270:	from all lookup 52
9000:	from all fwmark 0x2024 goto 9002
9001:	from all fwmark 0x2023 lookup 2022
9002:	from all nop
32766:	from all lookup main
32767:	from all lookup default
32768:	not from all fwmark 0x2024 lookup 2022
```

### Routing Table

#### Tailscale

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

#### sing-box

```console
$ ip route list table 2022
0.0.0.0/2 via 172.19.0.2 dev singbox-tun
64.0.0.0/3 via 172.19.0.2 dev singbox-tun
96.0.0.0/6 via 172.19.0.2 dev singbox-tun
100.0.0.0/10 via 172.19.0.2 dev singbox-tun
100.128.0.0/9 via 172.19.0.2 dev singbox-tun
101.0.0.0/8 via 172.19.0.2 dev singbox-tun
102.0.0.0/7 via 172.19.0.2 dev singbox-tun
104.0.0.0/5 via 172.19.0.2 dev singbox-tun
112.0.0.0/4 via 172.19.0.2 dev singbox-tun
128.0.0.0/1 via 172.19.0.2 dev singbox-tun
```

## Reference

[Can I use Tailscale alongside other VPNs?](https://tailscale.com/docs/reference/faq/other-vpns#split-tunnels)

[Troubleshoot TCP connection issues between two devices](https://tailscale.com/docs/reference/troubleshooting/network-configuration/tcp-connection-two-devices)

[List of reserved IP addresses](https://en.wikipedia.org/wiki/List_of_reserved_IP_addresses)

[IP routes installed by Tailscale](https://tailscale.com/docs/reference/troubleshooting/network-configuration/tailscale-ip-routes)

[Netfilter hooks](https://wiki.nftables.org/wiki-nftables/index.php/Netfilter_hooks)

[Ruleset debug/tracing](https://wiki.nftables.org/wiki-nftables/index.php/Ruleset_debug/tracing)
