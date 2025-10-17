# disable-ipv6-openwrt
```bash
uci set 'network.lan.ipv6=0'
uci set 'network.wan.ipv6=0'
uci set 'dhcp.lan.dhcpv6=disabled'

# Disable RA and DHCPv6 so no IPv6 IPs are handed out
uci -q delete dhcp.lan.dhcpv6
uci -q delete dhcp.lan.ra

# Disable the LAN delegation
uci set network.lan.delegate="0"

# Delete the IPv6 ULA Prefix
uci -q delete network.globals.ula_prefix

# Disable odhcpd
/etc/init.d/odhcpd disable
/etc/init.d/odhcpd stop

# Enabling the filtering of AAAA records in DNS.
uci set dhcp.@dnsmasq[0].filter_aaaa='1'

# Save changes
uci commit

# Disable IPv6 support for network interfaces via sysctl
sysctl -w net.ipv6.conf.all.disable_ipv6=1
sysctl -w net.ipv6.conf.default.disable_ipv6=1
sysctl -w net.ipv6.conf.lo.disable_ipv6=1

echo 1 > /proc/sys/net/ipv6/conf/all/disable_ipv6

/etc/init.d/network restart
service dnsmasq restart
```
