# Basic VLAN Configuration
```
Switch> ena
Switch# show vlan brief (muestra las vlans en los puertos)
Switch# config t
Switch (config)# vlan “numero de vlan a crear”
Switch (config-vlan)#  name “nombre de vlan”
Switch (config-vlan# exit (para salir del modo configuration vlan y crear una nueva vlan)
Switch (config)# vlan “numero de vlan a crear”
Switch (config-vlan)# name “nombre de vlan”
Switch (config-vlan)#exit
Switch (config)# (presionamos ctrl+c para salir del modo config)
Switch# show vlan brief (para ver las vlans creadas)
```
1. Vlans del 1 al 1005 son las que podemos asignar se crean en vlan.dat
2. Vlan del 1002 al 1005 estan reservadas se crean en vlan.dat
3. Vlan del 1006 al 4096 son vlan extendidas y se crearn en running config

VTP es vlan trunking protocol, para resolver el problema de vtp debemos de ponerlo en modo transparente, con esto podemos crear vlans entre el 1006 y 4096

```
Switch (config)# vtp mode transparent
setting device to VTP transparent mode for vlans
```

![basic vlan configuration](https://github.com/btock/Cisco-network-tips/assets/14008255/6782b12c-1a4f-456c-bf87-4e5726c24b6c)
