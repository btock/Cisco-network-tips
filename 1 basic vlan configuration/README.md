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


![basic vlan configuration](https://github.com/btock/Cisco-network-tips/assets/14008255/6782b12c-1a4f-456c-bf87-4e5726c24b6c)
