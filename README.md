# tools
some tools

# DHCP
apt install isc-dhcp-server -y

nano /etc/dhcp/dhcpd.conf  
`#option domain-name "example.com";`  
`#option domain-name-servers x.x.x.x;`  
default-lease-time 3600;  
max-lease-time 7200;  
authoritative;  
subnet 192.168.0.0 netmask 255.255.255.0 {  
&nbsp;&nbsp;&nbsp;&nbsp;option routers 192.168.0.1;  
&nbsp;&nbsp;&nbsp;&nbsp;option subnet-mask 255.255.255.0;  
&nbsp;&nbsp;&nbsp;&nbsp;option domain-name-servers 1.1.1.1, 8.8.8.8;  
&nbsp;&nbsp;&nbsp;&nbsp;range 192.168.0.100 192.168.0.200;  
}

nano /etc/default/isc-dhcp-server  
add interface

systemctl restart isc-dhcp-server.service  

dhcp-lease-list  

# NAT
apt install ufw

nano /etc/ufw/sysctl.conf  
net/ipv4/ip_forward=1  
net/ipv6/conf/default/forwarding=1  

nano /etc/sysctl.conf  
net.ipv4.ip_forward=1  
net.ipv6.conf.all.forwarding=1  

nano /etc/default/ufw  
DEFAULT_FORWARD_POLICY="ACCEPT"  

nano /etc/ufw/before.rules  
*nat  
:POSTROUTING ACCEPT [0:0]  

-A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE  

COMMIT

ufw allow 1:65535/tcp  
ufw allow 1:65535/udp  
ufw enable  

# APT-PROXY
nano /etc/apt/apt.conf.d/proxy.conf  
Acquire::http::Proxy "http://http-proxy.sero.gic.ericsson.se:8080";  
Acquire::https::Proxy "http://http-proxy.sero.gic.ericsson.se:8080";  
