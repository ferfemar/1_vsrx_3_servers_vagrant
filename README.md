Simple vSRX w/ two servers topology
===================================

A Vagrant topology using official Juniper vSRX image and two Alpine Linux servers for testing.

Topology
========
```
          eth0  +-------------+
        +-------+     SRV1    |
         to host|             |
                +-------------+
                      .2| 10.10.10.0/24
                      .1|
ge+0/0/0 +----------------------------+
 +-------+            VSRX1           |
  to host|    ge-0/0/1       ge-0/0/2 |
         +----------------------------+
 10.20.20.0/24 .1|              .1| 10.30.30.0/24
               .2|              .2|
   eth0  +-------------+  +-------------+ eth0
 +-------+     SRV2    |  |     SRV3    +-------
  to host|             |  |             |to host
         +-------------+  +-------------+

```

Requirements
============

- Vagrant with plugin junos-vagrant
- Virtualbox

Usage
=====

Clone the distro 

```
git clone https://github.com/ferfemar/1_vsrx_3_servers_vagrant.git
```

Run the topology

```
vagrant up
```

Connect to devices

```
vagrant ssh srv1
vagrant ssh srv2
vagrant ssh srv3
vagrant ssh vsrx1
```
