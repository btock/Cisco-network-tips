# TRUNK PORTS

Allow devices attached to different switches but assigned to the same vlan group to communicate with one another across the switched topology
Permita que los dispositivos conectados a diferentes conmutadores pero asignados al mismo grupo vlan se comuniquen entre sí a través de la topología conmutada
No son miembros de alguna vlan
Dirigie el trafico de un switch a switch por todas las vlans

## Crear un trunk puerto

Para que un Puerto sea trunk se requiere conectarse a otro switch, es switch a switch
Switch> ena
Switch# show vlan brief (muestra las vlans en los puertos)
Switch# config t
Switch (config)# interface fa0/24  (aqui elegimos el rango de puertosdonde se va a asignar la vlan, como best practice se elige el ultimo puerto)
Switch (config-if)# switchport mode trunk (para eligir la opcion trunk en el puerto 24
Switch (config-if)# “ctrl+c”
Switch# show interface trunk (muestra todas las configuracion trunk en el switch
Esta configuración se debe de hacer en los dos switches en los dos puertos conectados

## Trunk Network Diagram
***
![Trunk network diagram](https://github.com/btock/Cisco-network-tips/assets/14008255/68225c8f-4ce4-4c52-8570-d2db7acd96b3)

## 1- configuracion base de switch
***
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
***
configuration should be performed on switch 0 (TrunkSW) and switch 1 (Trunk2Sw)
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
TrunkSw>ena
Password: 
TrunkSw#config t
Enter configuration commands, one per line.  End with CNTL/Z.
TrunkSw(config)#interface range fa0/1-2
TrunkSw(config-if-range)#switchport mode access
TrunkSw(config-if-range)#switchport access vlan 10
TrunkSw(config-if-range)#exit
TrunkSw(config)#interface range fa0/10-11
TrunkSw(config-if-range)#switchport mode access
TrunkSw(config-if-range)#switchport access vlan 20
TrunkSw(config-if-range)#exit
TrunkSw(config)#exit
TrunkSw#
%SYS-5-CONFIG_I: Configured from console by console
TrunkSw#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
TrunkSw#
TrunkSw#sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/3, Fa0/4, Fa0/5, Fa0/6
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
10   RHleft                           active    Fa0/1, Fa0/2
20   ITright                          active    Fa0/10, Fa0/11
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
10   enet  100010     1500  -      -      -        -    -        0      0
20   enet  100020     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------

Remote SPAN VLANs
------------------------------------------------------------------------------

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------
TrunkSw#
```
## 3- Trunk creation and interface assignation
***
switch 0 (TrunkSW)
```
TrunkSw>ena
Password: 
TrunkSw#config t
Enter configuration commands, one per line.  End with CNTL/Z.
TrunkSw(config)#int fa0/24
TrunkSw(config-if)#switchport trunk allowed vlan 10,20
TrunkSw(config-if)#switchport mode trunk
TrunkSw(config-if)#do sh interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/24      on           802.1q         trunking      1

Port        Vlans allowed on trunk
Fa0/24      10,20

Port        Vlans allowed and active in management domain
Fa0/24      10,20

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/24      20

TrunkSw(config-if)#exit
TrunkSw(config)#exit
TrunkSw#
%SYS-5-CONFIG_I: Configured from console by console

TrunkSw#copy runni
TrunkSw#copy running-config startu
TrunkSw#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
TrunkSw#
```
switch 1 (Trunk2Sw)
```
Trunk2Sw>ena
Password: 
Trunk2Sw#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Trunk2Sw(config)#int fa0/24
Trunk2Sw(config-if)#switchport trunk allowed vlan 10,20
Trunk2Sw(config-if)#switchport mode trunk
Trunk2Sw(config-if)#do sh interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/24      on           802.1q         trunking      1

Port        Vlans allowed on trunk
Fa0/24      10,20

Port        Vlans allowed and active in management domain
Fa0/24      10,20

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/24      20

Trunk2Sw(config-if)#exit
Trunk2Sw(config)#exit
Trunk2Sw#
%SYS-5-CONFIG_I: Configured from console by console

Trunk2Sw#copy runni
Trunk2Sw#copy running-config startu
Trunk2Sw#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
Trunk2Sw#
```
## 4- Trunk interface Validation 
***
#### Trunk vlan 10
Ping succesfully between PC1 to PC4 & PC5 (VLAN10)

![PC1 to PC5](https://github.com/btock/Cisco-network-tips/assets/14008255/0abb2bb6-dee9-4290-b865-5360b8819aec)

#### Trunk vlan 20
Ping succesfully between PC3 to PC6 & PC7 (VLAN20)

![PC3 to PC6](https://github.com/btock/Cisco-network-tips/assets/14008255/7744021e-8fed-48de-8ab1-3c58b14bb6a1)
