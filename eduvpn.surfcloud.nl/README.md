# Introduction

This folder describes the specific documentation for `eduvpn.surfcloud.nl`. It 
is assumed the generic installation instructions are followed from this 
repository. This document lists the changes compared to those instructions.

The hostname is `eduvpn.surfcloud.nl`, the external interface on the actual
deployed VM is `ens32`.

# SAML

Following the generic SAML instructions works for this instance as SURFconext
is used as IdP.

# VPN Configuration

There are both public IPv4 and IPv6 addresses available:

- `195.169.120.0/23`
- `2001:610:450::/48`

These are configured in `/etc/openvpn/server.conf`:

    server 195.169.120.0 255.255.255.0
    server-ipv6 2001:610:450:4242::/64

And `/etc/openvpn/server-tcp.conf`:

    server 195.169.121.0 255.255.255.0
    server-ipv6 2001:610:450:4343::/64

The DNS servers are configured like this in both server configuration files:

    push "dhcp-option DNS 195.169.124.124"
    push "dhcp-option DNS 192.87.36.36"
    push "dhcp-option DNS 2001:610:3:200a:192:87:36:36"
    push "dhcp-option DNS 2001:610:0:800b:195:169:124:124"

The IPv6 DNS addresses are not picked up by all OpenVPN clients, it seems 
[Viscosity](https://www.sparklabs.com/viscosity/) on Windows picks this up, 
but not OpenVPN on Windows. Other clients are untested as of now.

Two OpenVPN instances are available, one on `udp/1194` and one on `tcp/443`.

# Firewall

No NAT is used, so the firewall is a bit different.

    $ sudo cp iptables /etc/sysconfig/iptables
    $ sudo cp ip6tables /etc/sysconfig/ip6tables
    $ sudo systemctl restart iptables
    $ sudo systemctl restart ip6tables

Note that `ens32` is the external interface!

# Branding

The User and Admin Portal have "eduvpn" branding, which includes a logo in 
the top right.

## User Portal

### Templates

Install the templates:

    $ sudo mkdir -p /etc/vpn-user-portal/views
    $ sudo cp vpn-user-portal/*.twig /etc/vpn-user-portal/views

## Admin Portal

### Templates

Install the templates:

    $ sudo mkdir -p /etc/vpn-admin-portal/views
    $ sudo cp vpn-admin-portal/*.twig /etc/vpn-admin-portal/views

## CSS

Install the CSS files:

    $ sudo mkdir -p /var/www/html/css
    $ sudo cp *.css /var/www/html/css
    $ sudo cp bootstrap/*.css /var/www/html/css

## Images

Instal the images:

    $ sudo mkdir -p /var/www/html/img
    $ sudo cp *.png /var/www/html/img