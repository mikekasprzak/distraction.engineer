+++
title = "Networking, Routing, and Firewalls"
draft = true
+++
This is a collection of general notes on networking, as it can be quite complicated and has a lot of terms.

## Networking and Routing

### Network Layers
![](LayersOfOSI3.png)

Source: <https://www.guru99.com/layers-of-osi-model.html>

Generally this translates into discussions of Layer 2 (Link), Layer 3 (Network), and Layer 7 (AKA Layer 4-7) switching.

### Network Protocol Layers by OSI
Reference: <https://en.wikipedia.org/wiki/List_of_network_protocols_(OSI_model)>, <https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/>

* Layer 1 &mdash; **Physical Layer**
  * The medium of transport
  * Cables, Radio (WiFi), etc
  * A "hub" is considered a Layer 1 switch, since it simply mirrors the input to all other ports
    * This causes collisions, reducing overall throughput
* Layer 2 &mdash; **Data Link Layer**
  * Data is routed by MAC addresses (Media Access Control)
  * Most data is wrapped in an Ethernet Frame
    * Frames: <https://en.wikipedia.org/wiki/Frame_(networking)>
    * Ethernet Frames: <https://en.wikipedia.org/wiki/Ethernet_frame#Structure>
      * Header
      * SRC and DEST MAC addresses
      * optional VLAN port(s)
      * payload length and type: Discovery (ARP, NDP), IP (IPv4 or IPv6), VLAN (optional), PPPoE (optional)
      * payload (data)
      * 32bit CRC
      * Padding (it's standard to insert padding between packets)
    * Layer 2 data is routed by MAC address
      * With typical IPv4 networking, a device knows its IP, its network (subnet), and the next-hop IP (router/gateway)
      * If receiver is not known, we make an ARP request (broadcast/many-cast) with a destination MAC of FF:FF:FF:FF:FF:FF.
        * The request will be forwarded by the switch to all ports.
        * Once found, the responder will send a response ARP directly to the broadcaster (i.e. unicast/one-cast)
      * Switches store a table of MAC addresses: i.e. which port leads to which MAC
    * By default, the Frame MTU (Maximum Transmission Unit) is 1500 bytes (octets) per payload
      * Some of that payload is eaten up by protocols like IP, TCP, and UDP
    * With Jumbo Frames, the MTU can be 9000 bytes
      * Reference: <https://en.wikipedia.org/wiki/Jumbo_frame>
      * Jumbo Frames improve throughput by requiring fewer packets to send data
      * Jumbo Frames must be enabled on every device
      * PMTUD can be used to determine the MTU of a device
        * <https://en.wikipedia.org/wiki/Path_MTU_Discovery>
        * PMTUD will fail if ICMP is disabled or blocked
        * On IPv4 networks, PMTUD works by first setting the "don't fragment" bit of an outgoing packet
          * If a packet is too large, the packet is dropped and an ICMP response is sent saying "fragmentation needed"
          * Sending retries with a smaller MTU
          * This repeats until an acceptable MTU is found between the two clients
        * IPv6 doesn't support packet fragmentation, but the process is otherwise the same
          * If a packet is too large, an ICMP response "Packet Too Big" is sent including the device MTU size
          * In theory subsequent packets will be correct, but this process repeats until an acceptable MTU is found
    * Local hardware support:
      * TP-Link TL-SG108E (8 port smart switch) supports **16 KB** Jumbo Frames, storage of 4K MAC addresses, and has 1.5 Mbits of packet buffer (196608 KB)
        * <https://www.tp-link.com/ca/business-networking/easy-smart-switch/tl-sg108e/#specifications>
      * TP-Link TL-SG105E (5 port smart switch) supports 15 KB Jumbo Frames, storage of 2K MAC addresses, and has 1.5 Mbits of packet buffer (196608 KB)
        * <https://www.tp-link.com/ca/business-networking/easy-smart-switch/tl-sg105e/#specifications>
      * TP-Link TL-SG108 (8 port unmanaged) supports 15 KB Jumbo Frames, storage of 4K MAC addresses, and has 1.5 Mbits of packet buffer (196608 KB)
        * <https://www.tp-link.com/ca/home-networking/soho-switch/tl-sg108/v1/#specifications>
      * TP-Link TL-SG105 (5 port unmanaged) supports **16 KB** Jumbo Frames, storage of 2K MAC addresses, and has 1 Mbits of packet buffer (131072 KB)
        * <https://www.tp-link.com/ca/home-networking/soho-switch/tl-sg105/v6/#specifications>
      * TP-Link POE switches are about the same
        * POE unmanaged 5 port: <https://www.tp-link.com/ca/business-networking/poe-switch/tl-sf1005p/>
        * POE+ unmanaged 5 port: <https://www.tp-link.com/ca/business-networking/poe-switch/tl-sg1005p/>
        * POE+ Smart Switch 5 port: <https://www.tp-link.com/ca/business-networking/poe-switch/tl-sg105pe/>
        * POE+ Smart Switch 8 port: <https://www.tp-link.com/ca/business-networking/poe-switch/tl-sg108pe/>
  * WiFi Frames (802.11) are a different than Ethernet Frames
    * Wireless Access Points (AP's) convert WiFi Frames to Ethernet Frames
      * <https://en.wikipedia.org/wiki/802.11_Frame_Types>
  * PPP and PPPoE (Point to Point over Ethernet)
    * <https://en.wikipedia.org/wiki/Point-to-Point_Protocol>
    * <https://en.wikipedia.org/wiki/Point-to-Point_Protocol_over_Ethernet>
    * Adds 8 bytes of payload overhead for all internet traffic sent over DSL lines
      * Effectively makes the internet MTU 1492 bytes rather than 1500
        * May trigger packet fragmenting, which degrades throughput
        * This can be especially problematic if ICMP is blocked (no "please fragment" requests)
        * Enabling Jumbo Frame support between Ethernet interfaces (modem and router) can resolve this
    * PPTP is PPP Tunneling. It's insecure. Do not use.
  * L2TP (Layer 2 Tunneling Protocol)
    * <https://en.wikipedia.org/wiki/Layer_2_Tunneling_Protocol>
    * Contained within a UDP packet for compatibility and to avoid "TCP problems"
      * Odd that it's still considered a Layer 2 protocol despite being built on UDP
    * Often paired with IPSec (Layer 3), as it alone does not provide payload encryption
    * Used by both Cable and DSL **resellers** to route traffic over a 3rd party providers network
    * Once established, the tunnel routes raw packets
    * Recommended alternatives include IKEv2 or WireGuard (or OpenVPN over UDP)
      * Conflicting information about security of IKEv2 due to an NSA paper
      * My brief look said something about dictionary attacks, i.e. bad passwords
      * <https://en.wikipedia.org/wiki/Internet_Key_Exchange>
* Layer 3 &mdash; **Network Layer**
  * Data is routed by some higher-level representation, such as IP address (Internet Protocol) and port
  * IPv4, IPv6, NAT, ICMP (Ping), ARP (again), RIP, IPSec (tunnel)
  * ICMP
    * Should I block ICMP? No, but... <http://shouldiblockicmp.com/>
      * <https://www.reddit.com/r/networking/comments/l3k9t3/icmp_firewall_best_practice/>
      * LAN traffic definitely not
      * WAN traffic, if you're a home
    * MikroTik recommends allowing certain protocols through, and notes that the Linux kernel already rate limits to 100pps
      * <https://help.mikrotik.com/docs/display/ROS/Building+Advanced+Firewall>
  * ARP is considered a Layer 2+3 protocol
* Layer 4 &mdash; **Transport Layer**
  * TCP, UDP, NetBIOS (Windows device discovery), QUIC (UDP variant for HTTP/3)
  * TCP/IP and UDP/IP are considered a Layer 3+4 protocol of the Ethernet standard (Layer 2)
* Layer 5 &mdash; **Session Layer**
* Layer 6 &mdash; **Presentation Layer**
* Layer 7 &mdash; **Application Layer**
  * Full application-level implementations of protocols, built on lower level building blocks


## Firewalls
Firewalls limit traffic. Typically this is IP traffic, but on local networks this can also include ARP traffic.

Your router has a firewall, but individual machines can have them as well.

* IPv4 (L4) - 32bit IP addresses - `iptables --list`
* IPv6 (L4) - 128bit IP addresses - `ip6tables --list`
* ARP (L2-3) - 48bit MAC addresses - `arptables --list`
  * Used for device discovery on an IPv4 LAN, when no IP addresses are known
