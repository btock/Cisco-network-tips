# Router on a Stick
***
## 1- configuracion base de switch
***
#### Switch Base configuration (Switch1) 
- usuario y contraseña
  * user= beto
  * pass= ciscotest
- MOTD
  * this is bto's company switch, kindly Stay out!
- enable secret password
  * pass= ciscotest
- domain name
  * beto.com
- hostname
  * Switch1
- ssh version 2
- Secure console port
  * pass= ciscotest
- secure vty 0-4
  * pass= ciscotest

```
Switch>ena
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname Switch1
Switch1(config)#banner motd #this is bto's company switch, kindly stay out!#
Switch1(config)#username beto password ciscotest
Switch1(config)#enable secret ciscotest
Switch1(config)#line console 0
Switch1(config-line)#password ciscotest
Switch1(config-line)#login
Switch1(config-line)#exit
Switch1(config)#line vty 0 4
Switch1(config-line)#password ciscotest
Switch1(config-line)#login
Switch1(config-line)#exit
Switch1(config)#ip domain name beto.com
Switch1(config)#crypto key generate rsa
The name for the keys will be: Switch1.beto.com
Choose the size of the key modulus in the range of 360 to 4096 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

Switch1(config)#ip ssh version 2
*Mar 1 0:32:0.977: %SSH-5-ENABLED: SSH 1.99 has been enabled
Switch1(config)#exit
Switch1#
%SYS-5-CONFIG_I: Configured from console by console

Switch1#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
Switch1#
```
## 2- VLAN creation and interface assignation
***
1- VLAN 10
  - assigned to interface fa0/1, fa0/2
  - name HRvlan

2- VLAN 20
  - assigned to interface fa0/10, fa0/11
  - name HRvlan

3- vlan 30
  - assigned to interface fa0/20, fa0/21
  - name HRvlan
```
Switch1>ena
Password: 
Switch1#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch1(config)#vlan 10
Switch1(config-vlan)#name HRvlan
Switch1(config-vlan)#vlan 20
Switch1(config-vlan)#name ITvlan
Switch1(config-vlan)#vlan 30
Switch1(config-vlan)#name ENGvlan
Switch1(config-vlan)#exit
Switch1(config)#interface range fa0/1-2
Switch1(config-if-range)#switchport mode access
Switch1(config-if-range)#switchport access vlan 10
Switch1(config-if-range)#exit
Switch1(config)#interface range fa0/10-11
Switch1(config-if-range)#switchport mode access
Switch1(config-if-range)#switchport access vlan 20
Switch1(config-if-range)#exit
Switch1(config)#interface range fa0/20-21
Switch1(config-if-range)#switchport mode access
Switch1(config-if-range)#switchport access vlan 30
Switch1(config-if-range)#exit
Switch1(config)#exit
Switch1#
%SYS-5-CONFIG_I: Configured from console by console
sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/3, Fa0/4, Fa0/5, Fa0/6
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
10   HRvlan                           active    Fa0/1, Fa0/2
20   ITvlan                           active    Fa0/10, Fa0/11
30   ENGvlan                          active    Fa0/20, Fa0/21
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
10   enet  100010     1500  -      -      -        -    -        0      0
20   enet  100020     1500  -      -      -        -    -        0      0
30   enet  100030     1500  -      -      -        -    -        0      0
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
Switch1#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
Switch1#
```
## 3- Trunk creation and interface assignation to switch
***
switch1 should have the trunk interface where the router is connected
- Gi0/1
```
Switch1>ena
Password: 
Switch1#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch1(config)#int Gi0/1
Switch1(config-if)#switchport trunk allowed vlan 10,20,30
Switch1(config-if)#switchport mode trunk
Switch1#sh int Gi0/1 switchport
Name: Gig0/1
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: down
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 1 (default)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 10,20,30
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
```
## 4- Router basic configuration
***
1- hostname
  * Router1
    
2- usuario y contraseña
  * user= beto
  * pass= ciscotest

3- MOTD
  * this is bto's company switch, kindly Stay out!

4- enable secret password
  * pass= ciscotest

5- domain name
  * beto.com

6- ssh version 2

7- Secure console port
  * pass= ciscotest
8- secure vty 0-4
  * pass= ciscotest

9- no ip domain lookup 
  * Router(config)# no ip domain-lookup

10- Interface Gi0/0/0
  * Enable
      

