# DEMO_ANSIBLE_1
This is a very simple project.
In my lab I use two vMX and Ubuntu Server with ansible and jsnapy.
# ansible --version
ansible 2.3.1.0
 python version = 2.7.12 (default, Jul 18 2016, 15:00:48) [GCC 4.6.3]
# jsnapy --version
JSNAPy version: 1.2.1.dev0
# ansible-galaxy --version
ansible-galaxy 2.3.1.0

In my lab I had two vMX with the next versions:
14.1R1.10 and 16.2R2.8.
In both equipments I had the next configuration at the services hierarchy.

First I checked if there any issue in the netconf connetion between the server and routers.
From Serveer:
# ssh -p 830 -s 192.168.111.3 netconf 
Password:
<!-- No zombies were killed during the creation of this user interface -->
<!-- user root, class super-user -->
<hello xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <capabilities>
    <capability>urn:ietf:params:netconf:base:1.0</capability>
    <capability>urn:ietf:params:netconf:capability:candidate:1.0</capability>
    <capability>urn:ietf:params:netconf:capability:confirmed-commit:1.0</capability>
    <capability>urn:ietf:params:netconf:capability:validate:1.0</capability>
    <capability>urn:ietf:params:netconf:capability:url:1.0?scheme=http,ftp,file</capability>
    <capability>urn:ietf:params:xml:ns:netconf:base:1.0</capability>
    <capability>urn:ietf:params:xml:ns:netconf:capability:candidate:1.0</capability>
    <capability>urn:ietf:params:xml:ns:netconf:capability:confirmed-commit:1.0</capability>
    <capability>urn:ietf:params:xml:ns:netconf:capability:validate:1.0</capability>
    <capability>urn:ietf:params:xml:ns:netconf:capability:url:1.0?protocol=http,ftp,file</capability>
    <capability>http://xml.juniper.net/netconf/junos/1.0</capability>
    <capability>http://xml.juniper.net/dmi/system/1.0</capability>
  </capabilities>
  <session-id>4665</session-id>
</hello>
]]>]]>

In the vMX:
@vMX1> show system connections | match 830    
tcp4       0      0  192.168.111.3.830                             192.168.111.130.34516                         ESTABLISHED
tcp4       0      0  *.830                                         *.*                                           LISTEN

Then, I created two files called hosts and ansible.cnf.

