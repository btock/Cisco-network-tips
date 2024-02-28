# Basic VLAN Configuration
- Switch> ena
- Switch# show vlan brief (muestra las vlans en los puertos)
- Switch# config t
- Switch (config)# vlan “numero de vlan a crear”
- Switch (config-vlan)#  name “nombre de vlan”
- Switch (config-vlan# exit (para salir del modo configuration vlan y crear una nueva vlan)
- Switch (config)# vlan “numero de vlan a crear”
- Switch (config-vlan)# name “nombre de vlan”
- Switch (config-vlan)#exit
- Switch (config)# (presionamos ctrl+c para salir del modo config)
- Switch# swho vlan brief (para ver las vlans creadas)

I designed this **network configuration for the network of three companies**, given some constraints. This problem was presented in our  Computer Networks Lab (CL307) Final. The main role was to **subnet the IP addresses** correctly.

Its an interesting problem demonstrating the concepts of **Classless IP Subnetting** and using **RIPv2 Protocol**. I am sharing this **working solution** so that it might be of help to others looking to learn these concepts with a practical real world example.

![basic vlan configuration](https://github.com/btock/Cisco-network-tips/assets/14008255/6782b12c-1a4f-456c-bf87-4e5726c24b6c)
