TRUNK PORTS

Allow devices attached to different switches but assigned to the same vlan group to communicate with one another across the switched topology
Permita que los dispositivos conectados a diferentes conmutadores pero asignados al mismo grupo vlan se comuniquen entre sí a través de la topología conmutada
No son miembros de alguna vlan
Dirigie el trafico de un switch a switch por todas las vlans

Crear un trunk puerto

Para que un Puerto sea trunk se requiere conectarse a otro switch, es switch a switch
Switch> ena
Switch# show vlan brief (muestra las vlans en los puertos)
Switch# config t
Switch (config)# interface fa0/24  (aqui elegimos el rango de puertosdonde se va a asignar la vlan, como best practice se elige el ultimo puerto)
Switch (config-if)# switchport mode trunk (para eligir la opcion trunk en el puerto 24
Switch (config-if)# “ctrl+c”
Switch# show interface trunk (muestra todas las configuracion trunk en el switch
Esta configuración se debe de hacer en los dos switches en los dos puertos conectados

## 1- configuracion base de switch
Primero configuramos la configuración base y habilitamos ssh en el switch 
- Config base;
- usuario y contraseña
- enable secret password
- domain name
- hostname
- ssh version 2
- Secure console port
- secure vty 0-4
  
```
Switch>
Switch>ena
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname TrunkSw
TrunkSw(config)#banner motd #this is bto's company switch, kindly Stay out!#
TrunkSw(config)#username beto password ciscotest
TrunkSw(config)#enable secret ciscotest
TrunkSw(config)#line console 
TrunkSw(config)#line console 0
TrunkSw(config-line)#password ciscotest
TrunkSw(config-line)#login
TrunkSw(config-line)#exit
TrunkSw(config)#line vty 0 4
TrunkSw(config-line)#password ciscotest
TrunkSw(config-line)#login
TrunkSw(config-line)#exit
TrunkSw(config)#ip d
TrunkSw(config)#ip domain name beto.com
TrunkSw(config)#cryp
TrunkSw(config)#crypto key gene
TrunkSw(config)#crypto key generate rsa
The name for the keys will be: TrunkSw.beto.com
Choose the size of the key modulus in the range of 360 to 4096 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.
How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
TrunkSw(config)#ip ssh version 2
TrunkSw(config)#exit
TrunkSw#
%SYS-5-CONFIG_I: Configured from console by console
TrunkSw#copy running-config  startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
TrunkSw#
```
## 2- VLAN creation and interface assignation
```
TrunkSw#config t
Enter configuration commands, one per line.  End with CNTL/Z.
TrunkSw(config)#vlan 10
TrunkSw(config-vlan)#name RHleft
TrunkSw(config-vlan)#vlan 20
TrunkSw(config-vlan)#name ITright
TrunkSw(config-vlan)#exit
TrunkSw(config)#exit
TrunkSw#
%SYS-5-CONFIG_I: Configured from console by console
copy running-config  startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
TrunkSw#
```
