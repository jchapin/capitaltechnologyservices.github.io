---
layout: post
title: "PPTP on OS X Just One Last Time"
title_short: "PPTP on OS X Just One Last Time"
excerpt: >
  Apple killed off its support for PPTP VPNs when it released OS X in September
  of 2016. There was one application we had to maintain where the only way to
  connect and update it was via this out-of-favor protocol. Here's how to get
  in and get the job done over PPTP one last time.
---

{{ page.excerpt }}

## The Problem

First, credit for this solution should go to the following post:
[https://smallhacks.wordpress.com/2016/12/20/pptp-on-osx-sierra/](https://smallhacks.wordpress.com/2016/12/20/pptp-on-osx-sierra/)

We only had to make a couple of tweaks to the ppp configuration provided on that
page in order to get the job done and they're likely specific to our
circumstances.

Second, you should really get off of PPTP VPNs as soon as practicable. The most
common authentication protocol used on them, MS-CHAPv2, has been known to be
vulnerable to attacks since 1999 and in 2012 Moxie Marlinspike released a tool
that could be used in combination with a brute force attack to decrypt traffic
over the VPN tunnel.

[https://github.com/moxie0/chapcrack](https://github.com/moxie0/chapcrack)

So, make plans to retire that VPN now and move to an IPSec or SSL VPN.

Write the following file to ```/etc/ppp/peers/pptpvpn```:

```
plugin PPTP.ppp
noauth
# logfile /tmp/ppp.log
remoteaddress AAA.BBB.CCC.DDD # Address of the VPN Server
redialcount 1
redialtimer 5
idle 1800
mru 1368
mtu 1368
receive-all
novj 0:0
ipcp-accept-local
ipcp-accept-remote
#noauth
refuse-eap
refuse-pap
refuse-chap-md5
user YOURUSERNAME
hide-password
mppe-stateless
mppe-128
looplocal
password YOURPASSWORD
nodetach
ms-dns 8.8.8.8
# used in ip-up script
ipparam gwvpn
debug
```

## The Modifications

We had to add the following lines to the configuration to help connect to our
client's network:

```
refuse-eap
```

We encountered errors related to Extensible Authentication Protocol, turning it
off allowed us to move on.

```
debug
```

It was useful to turn on debugging to trace down the cause of one particular
issue. At one site, the configuration requests being sent were not being
acknowledged. After 10 unacknowledged requests, the connection was dropped. So
turn on debugging and you should see requests and acknowledgements as well as
messages detailing the authentication process.

```
sent [LCP ConfReq...
rcvd [LCP ConfReq...
```

The likely cause of the unacknowledged requests was that the router we were
communicating through didn't support Generic Router Encapsulation (GRE). Since
we did not control that router, there was nothing to do but move to a site that
we could communicate from.

Connect to the VPN by running:

```
sudo pppd call pptpvpn
```

## After Connecting

When you connect successfully you should get a response like this in your
terminal:

```
pptp_wait_input: Address added. previous interface setting...
```

What you will need to do next, in order to make sure you're not just sending
requests out of the wrong interface, is to add a route to the host you need to
contact on the other network:

```
sudo route add -host WWW.XXX.YYY.ZZZ -interface ppp0
```

Then you should be able to browse, ssh, ping, or do whatever you need to with
the host on the other side.

## Well I could just buy a VPN client...

Fair enough. There are two vendors for VPN clients for OS X supporting PPTP. One
client costs $50 and the other vendor offers their software as an annual
subscription. Unfortunately, buying new software doesn't deal with the fact that
PPTP should be phased out. Save your money, use the underlying ppp client in OS
X to get connected, and then plan on replacing that old protocol.
