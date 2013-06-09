# SixArm.com »  <br> PeerGuardian Linux Setup

PeerGuardian Linux (pgl) is a privacy-oriented firewall application. 
It blocks connections to and from hosts specified in huge block lists,
of thousands or millions of IP ranges. 

PeerGuardian Linux is based on the Linux kernel netfilter framework and iptables.

See https://wiki.archlinux.org/index.php/PeerGuardian_Linux

We install PeerGuardian on some of our systems, and we do some simple setup
so that PeerGuardian works with the ports we want and our system services.

This repo has our various setup files, including pgl.conf.


### Configuration

We want to whitelist the ports for SSH, HTTP, HTTPS.

Edit /etc/pgl/pglcmd.conf:

    WHITE_TCP_OUT="ssh http https"
    

### Firewall

Create /etc/systemd/system/pgl.service pasting the following to 

Ensure that pgl only starts *after* all the rules have been properly set up. 
This example must be adapted to work with other firewalls (ufw, shorewall, etc.):

Edit /etc/systemd/system/pgl.service:

    .include /usr/lib/systemd/system/pgl.service

    [Unit]
    After=iptables.service


### Server

systemd initialization of the system means that it's quite possible for
a server to be briefly unprotected, prior to pgl launch. To ensure adequate
protection, we create a service file named after the original server:

Edit /etc/systemd/system/httpd.service:

    .include /usr/lib/systemd/system/httpd.service

    [Unit]
    Wants=pgl.service
    After=pgl.service


### LAN

By default, pgl blocks traffic on the local IPv4 addresses.

We want local traffic, so we whitelist it.

Edit /etc/pgl/pglcmd.conf:

    WHITE_IP_OUT="192.168.0.0/24"


### Contact

To contact us with your questions, comments, and suggestions:

  * http://sixarm.com
  * sixarm@sixarm.com
