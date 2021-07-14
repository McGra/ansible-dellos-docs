############
Installation
############

You can install Ansible roles on the control machine using Dell EMC devices.

Ansible modules
***************

Dell EMC Ansible modules for dellos6, dellos9, and dellos10 are part of the Ansible core. Install Ansible 2.3 or later to use these modules. To use OpenSwitch Ansible "opx_cps" module, install Ansible 2.7 or later. See `Ansible documentation <http://docs.ansible.com/ansible/intro_installation.html>`_ for more information.

Ansible roles
*************

Install all Dell EMC Ansible roles.

::

  ansible-galaxy install -r dellemc_roles.yml

where ``dellemc_roles.yml`` is defined as:

:: 

- src: Dell-Networking.dellos-aaa
- src: Dell-Networking.dellos-acl
- src: Dell-Networking.dellos-bgp
- src: Dell-Networking.dellos_bfd
- src: Dell-Networking.dellos-copy-config
- src: Dell-Networking.dellos-dcb
- src: Dell-Networking.dellos-dns
- src: Dell-Networking.dellos-ecmp
- src: Dell-Networking.dellos_fabric_summary
- src: Dell-Networking.dellos-flow-monitor
- src: Dell-Networking.dellos-image-upgrade
- src: Dell-Networking.dellos-interface
- src: Dell-Networking.dellos-lag
- src: Dell-Networking.dellos-lldp
- src: Dell-Networking.dellos-logging
- src: Dell-Networking.dellos_network_validation
- src: Dell-Networking.dellos-ntp
- src: Dell-Networking.dellos-prefix-list
- src: Dell-Networking.dellos-qos
- src: Dell-Networking.dellos-route-map
- src: Dell-Networking.dellos-sflow
- src: Dell-Networking.dellos-snmp
- src: Dell-Networking.dellos-system
- src: Dell-Networking.dellos_template
- src: Dell-Networking.dellos_uplink
- src: Dell-Networking.dellos-users
- src: Dell-Networking.dellos-vlan
- src: Dell-Networking.dellos-vlt
- src: Dell-Networking.dellos-vrf
- src: Dell-Networking.dellos-vrrp
- src: Dell-Networking.dellos_vxlan
- src: Dell-Networking.dellos-xstp

You can also install an individual Dell EMC Networking Ansible role using a single command. For example, to install the AAA role use ``ansible-galaxy install Dell-Networking.dellos.aaa``.

See `Ansible Galaxy <https://galaxy.ansible.com/Dell-Networking/>`_ for more information on Dell EMC Ansible roles.

Dell EMC devices
***************************

Dell EMC devices require minimal configuration to run Ansible playbooks.

OS6
---

#. Create a username and password for Ansible.

#. Configure the Management interface (static/dynamic IP address).

#. Enable the SSH server.

::

  console(config)# username admin password ansible@123
  console(config)# enable password ansible@123
  console(config)# interface  out-of-band
  console(conf-if)# ip address 10.16.148.79 255.255.255.0 10.16.148.254 
  console(conf-if)# exit
  console(config)# ip ssh  server 

OS9
---

1. Create a username and password for Ansible.

#. Configure the Management interface (static/dynamic IP address).

#. Enable the SSH server.

#. Set the maximum connection rate limit.

::

  Dell(config)# username ansible password ansible
  Dell(config)# enable password ansible
  Dell(config)# interface managementethernet 0/0
  Dell(conf-if-ma-0/0)# ip add 10.16.148.72/24
  Dell(conf-if-ma-0/0)# no shutdown 
  Dell(conf-if-ma-0/0)# exit
  Dell(config)# ip ssh server enable 
  Dell(config)# ip ssh connection-rate-limit 60

OS10
----

1. Create an Ansible username and password.

#. Configure the Management interface (static/dynamic IP address).

::

  OS10# config t
  OS10(config)# username ansible password ansible
  OS10(config)# interface mgmt 1/1/1
  OS10(conf-if-ma-1/1/1)# ip address 10.16.149.62/16
  OS10(conf-if-ma-1/1/1)# no shutdown
  OS10(conf-if-ma-1/1/1)# do commit
  OS10(conf-if-ma-1/1/1)# exit

> **NOTE**: SSH is enabled in OS10 by default.

OPX
----

1. Create an Ansible username and password.

#. Configure the Management interface (static/dynamic IP address).

::

  root@os10:/config/home/linuxadmin# useradd testuser
  root@os10:/config/home/linuxadmin# passwd testuser
  New password:
  Retype new password:
  passwd: password updated successfully
  root@os10:/config/home/linuxadmin# ifconfig eth0 10.16.148.123 netmask 255.255.255.0 up
  root@os10:/config/home/linuxadmin# route default gw 10.16.148.254 
