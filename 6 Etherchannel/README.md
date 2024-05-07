# ETHERCHANNEL
***
EtherChannel es una tecnología de agregación de enlaces que agrupa varios enlaces Ethernet físicos en un único enlace lógico. Se utiliza para proporcionar tolerancia a fallos, uso compartido de carga, mayor ancho de banda y redundancia entre switches, routers y servidores.

La tecnología de EtherChannel hace posible combinar la cantidad de enlaces físicos entre los switches para aumentar la velocidad general de la comunicación switch a switch.
***
## Network diagram

![etherchannel](https://github.com/btock/Cisco-network-tips/assets/14008255/3954519f-33bd-4193-8273-a5139dba6649)

***
## 1- Basic switch configuration (switch 1, switch 2, switch3)
***
1. Hostname
   - Switch1 & Switch2  
2. Banner MOTD
   - #banner motd #this is bto's company switch, kindly Stay out!#
3. User & Password
   - user: beto
   - pass: ciscotest
4. Security
   - enable secret ciscotest
   - line console 0 password ciscotest
   - Secure VTY 0 4 password ciscotest
   - SSH version 2 1024
5. Domain Name
   - beto.com
  
```
Switch>
Switch>ena
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname Switch1
Switch1(config)#banner motd #this is bto's company switch, kindly Stay out!#
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
*Mar 1 0:25:45.656: %SSH-5-ENABLED: SSH 1.99 has been enabled
Switch1(config)#exit
Switch1#
%SYS-5-CONFIG_I: Configured from console by console

Switch1#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
Switch1#
```
# 2- Trunk Port Assignation for etherchannel interfaces
***
## Switch1
```
Switch1>ena
Password: 

Switch1(config)#int range gi0/1-2, fa0/23-24
Switch1(config-if-range)#switchport mode trunk

Switch1(config-if-range)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/23, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/23, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to up

Switch1(config-if-range)#switchport nonegotiate
Switch1(config-if-range)#
```
## Switch2
```
Switch2>ena
Password: 
Switch2#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch2(config)#int range gi0/1-2, fa0/10-11
Switch2(config-if-range)#switchport mode trunk
Switch2(config-if-range)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/10, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/10, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/11, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/11, changed state to up

Switch2(config-if-range)#switchport nonegotiate
Switch2(config-if-range)#exit
Switch2(config)#exit
Switch2#
%SYS-5-CONFIG_I: Configured from console by console

Switch2#
```
## Switch3
***
```
Switch3>ena
Password: 
Switch3#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch3(config)#int range fa0/10-11, fa0/23-24
Switch3(config-if-range)#switchport mode trunk
Switch3(config-if-range)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/10, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/10, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/11, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/11, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/23, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/23, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to up

Switch3(config-if-range)#switchport nonegotiate
Switch3(config-if-range)#exit
Switch3(config)#exit
Switch3#
%SYS-5-CONFIG_I: Configured from console by console
Switch3#
```
# 3- Etherchannel configuration 
***
## 1- port channel 1
      * PaGP
        - mode desirable
      * Switch 1
        - interfaces Fa0/23, Fa0/24
      * Switch 3
        - interfaces Fa0/23, Fa0/24

PaGP (Port Aggregation Protocol)= Cisco Systems proprietary networking protocol used for automated link aggregation of Ethernet switch ports, known as EtherChannel. PAgP is owned by Cisco Systems

Below you will find details of the different modes of operation:

	* On: fuerza a los puertos a establecer el EtherChannel (Deshabilita PAgP).
	* Off: evita que los puertos establezcan un EtherChannel.
	* Auto: espera a recibir paquetes para negociar el EtherChannel.
	* Desirable: This mode enables the switch to actively negotiate to form a PaGP link.

## Switch 1
```
Switch1>ena
Password: 
Switch1#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch1(config)# int range fa0/23-24
Switch1(config-if-range)#shutdown
Switch1(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/23, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to down

Switch1(config-if-range)#channel-group 1 mode ?
  active     Enable LACP unconditionally
  auto       Enable PAgP only if a PAgP device is detected
  desirable  Enable PAgP unconditionally
  on         Enable Etherchannel only
  passive    Enable LACP only if a LACP device is detected

Switch1(config-if-range)#channel-group 1 mode desirable 
Switch1(config-if-range)#
Creating a port-channel interface Port-channel 1

Switch1(config-if-range)#no shutdown

%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to down
Switch1(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/23, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to up

%LINK-5-CHANGED: Interface Port-channel1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel1, changed state to up

Switch1(config-if-range)#
```

## Switch 3
```
Switch3>ena
Password: 
Switch3#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch3(config)#int range fa0/23-24
Switch3(config-if-range)#shutdown

%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down
Switch3(config-if-range)#channel-group 1 mode ?
  active     Enable LACP unconditionally
  auto       Enable PAgP only if a PAgP device is detected
  desirable  Enable PAgP unconditionally
  on         Enable Etherchannel only
  passive    Enable LACP only if a LACP device is detected

Switch3(config-if-range)#channel-group 1 mode desirable
Switch3(config-if-range)#
Creating a port-channel interface Port-channel 1

Switch3(config-if-range)#no shutdown

Switch3(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/23, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to up

%LINK-5-CHANGED: Interface Port-channel1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel1, changed state to up

Switch3(config-if-range)#
```

## A. Configure port channel 1 to become trunk 

### Switch 1
```
Switch1(config-if-range)#
Switch1(config-if-range)#int port-channel 1
Switch1(config-if)#switchport mode trunk
Switch1(config-if)#
```
### Switch 3
```
Switch3(config-if-range)#
Switch3(config-if-range)#int port-channel 1
Switch3(config-if)#switchport mode trunk
Switch3(config-if)#
```

## B. Verify port channel status on switch 1 and switch 3

### Switch 1
```
Switch1#
Switch1#sh etherchannel summary
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port


Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           PAgP   Fa0/23(P) Fa0/24(P) 

Switch1#
```
### Switch 3
```
Switch3#sh etherchannel summary
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port


Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           PAgP   Fa0/23(P) Fa0/24(P) 

Switch3#
```
## 2- port channel 2
      * LACP
      * Switch 1
        - interfaces Gi0/1, Gi0/2
        - Active
      * Switch 2
        - interfaces Gi0/1, Gi0/2
        - Passive

LACP (Link Aggregation Control Protocolo)=  es la opción «open» del protocolo. El funcionamiento, muy similar al de PAgP con la diferencia de que en este caso se asignan los roles a cada uno de los extremos basándose en la prioridad del sistema, que se conforma con 2 bytes de prioridad más 6 de MAC.

Los puertos son seleccionados y activados acorde el valor port priority, el valor más bajo indica mayor prioridad. En este caso se pueden definir hasta 16 enlaces por cada EtherChannel.

Below you will find details of the different modes of operation::

   * On: fuerza los puertos a establecer el EtherChannel (Deshabilita LACP).
   * Off: evita que se establezca el EtherChannel.
   * Passive: pone el puerto en espera de recibir paquetes LACP para negociar el EtherChannel.
   * Active: indicate that the switch actively tries to negotiate that link as LACP.

## Switch 1
```
Switch1>enable
Password: 
Switch1#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch1(config)#in range g0/1-2
Switch1(config-if-range)#shutdown


Switch1(config-if-range)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down

%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down

Switch1(config-if-range)#channel-group 2 mode active 
Switch1(config-if-range)#
Creating a port-channel interface Port-channel 2

Switch1(config-if-range)#no shutdown


Switch1(config-if-range)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up

Switch1(config-if-range)#
```
## Switch 2
```
Switch2>ena
Password: 
Switch2#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch2(config)#int range Gi0/1-2
Switch2(config-if-range)#channel-group 2 mode active 
Switch2(config-if-range)#
Creating a port-channel interface Port-channel 2

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up

%LINK-5-CHANGED: Interface Port-channel2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel2, changed state to up

Switch2(config-if-range)#no shutdown
Switch2(config-if-range)#
```
## A. configure and Verify port channel 2 as trunk 

### Switch 1
```
Switch1(config-if-range)#
Switch1(config-if-range)#int port-channel 2
Switch1(config-if)#switchport mode trunk
Switch1(config-if)#end
Switch1#
Switch1#sh int trunk
Port        Mode         Encapsulation  Status        Native vlan
Po1         on           802.1q         trunking      1
Po2         on           802.1q         trunking      1

Port        Vlans allowed on trunk
Po1         1-1005
Po2         1-1005

Port        Vlans allowed and active in management domain
Po1         1
Po2         1

Port        Vlans in spanning tree forwarding state and not pruned
Po1         1
Po2         1

Switch1#
```
### Switch 2
```
Switch2(config-if-range)#
Switch2(config-if-range)#int port-channel 2
Switch2(config-if)#switchport mode trunk
Switch2(config-if)#
Switch2#sh int trunk
Port        Mode         Encapsulation  Status        Native vlan
Po2         on           802.1q         trunking      1
Fa0/10      on           802.1q         trunking      1
Fa0/11      on           802.1q         trunking      1

Port        Vlans allowed on trunk
Po2         1-1005
Fa0/10      1-1005
Fa0/11      1-1005

Port        Vlans allowed and active in management domain
Po2         1
Fa0/10      1
Fa0/11      1

Port        Vlans in spanning tree forwarding state and not pruned
Po2         1
Fa0/10      none
Fa0/11      none
```
## B. Verify port channel status on switch 1 and switch 2

### Switch 1
```
Switch1#sh etherchannel summary
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port


Number of channel-groups in use: 2
Number of aggregators:           2

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           PAgP   Fa0/23(P) Fa0/24(P) 
2      Po2(SU)           LACP   Gig0/1(P) Gig0/2(P) 
Switch1#
```
### Switch 2
```
Switch2#sh etherchannel summary
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port


Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

2      Po2(SU)           LACP   Gig0/1(P) Gig0/2(P) 
Switch2#
```
## 3- port channel 3

      * Switch 2
        - interfaces Fa0/10, Fa0/11
        - passive
      * Switch 3
        - interfaces Fa0/10, Gi0/11
        - Active
	
Active = inidicates that you want the switch to use LACP unconditionally.

Passive = Enable LACP only if a LACP device is detected. Passive option indicates that you want the switch to use LACP only if another LACP device is detected.

## Switch 2
```
 Switch2#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch2(config)#int range Fa0/10-11
Switch2(config-if-range)#shutdown


Switch2(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/10, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/11, changed state to down

Switch2(config-if-range)#channel-group 3 mode passive
Switch2(config-if-range)#
Creating a port-channel interface Port-channel 3

Switch2(config-if-range)#
Switch2#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch2(config)#int range Fa0/10-11
Switch2(config-if-range)#shutdown


Switch2(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/10, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/11, changed state to down

Switch2(config-if-range)#channel-group 3 mode passive
Switch2(config-if-range)#
Creating a port-channel interface Port-channel 3

Switch2(config-if-range)# no shutdown
Switch2(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/10, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/11, changed state to up

Switch2(config-if-range)#
```
## Switch 3
```
Switch3#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch3(config)#int range Fa0/10-11
Switch3(config-if-range)#shutdown
Switch3(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/10, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/11, changed state to down

Switch3(config-if-range)#channel-group 3 mode active
Switch3(config-if-range)#
Creating a port-channel interface Port-channel 3

Switch3(config-if-range)#no shutdown


Switch3(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/10, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/11, changed state to up

Switch3(config-if-range)#
%LINK-5-CHANGED: Interface Port-channel3, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel3, changed state to up

Switch3(config-if-range)#
```
## A. Configure and Verify port channel 3 as trunk 

### Switch 2
```
Switch2#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch2(config)#int port-channel 3
Switch2(config-if)#switchport mode trunk
Switch2(config-if)#
```
### Switch 3
```
Switch3(config-if-range)#int port-channel 3
Switch3(config-if)#switchport mode trunk
Switch3(config-if)#
```
## B. Verify port channel status on switch 2 and switch 3

### Switch 2
```
Switch2#sh etherchannel summary
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port


Number of channel-groups in use: 2
Number of aggregators:           2

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

2      Po2(SU)           LACP   Gig0/1(P) Gig0/2(P) 
3      Po3(SU)           LACP   Fa0/10(P) Fa0/11(P)
```
### Switch 3
```
Switch3#sh etherchannel summary
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port


Number of channel-groups in use: 2
Number of aggregators:           2

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           PAgP   Fa0/23(P) Fa0/24(P) 
3      Po3(SU)           LACP   Fa0/10(P) Fa0/11(P)
```
# 4-  Veryfing port channel status on switch 1, 2 & 3

## Switch1
```
Switch1#sh etherchannel summary
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port


Number of channel-groups in use: 2
Number of aggregators:           2

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           PAgP   Fa0/23(P) Fa0/24(P) 
2      Po2(SU)           LACP   Gig0/1(P) Gig0/2(P) 
Switch1#sh spanning-tree
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.6433.5ED8
             Cost        12
             Port        27(Port-channel1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0090.2150.73CC
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Po2              Altn BLK 3         128.28   Shr
Po1              Root FWD 12        128.27   Shr

```
## Switch2
```
Switch2#sh etherchannel summary
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port
Number of channel-groups in use: 2
Number of aggregators:           2

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

2      Po2(SU)           LACP   Gig0/1(P) Gig0/2(P) 
3      Po3(SU)           LACP   Fa0/10(P) Fa0/11(P) 
Switch2#
Switch2#sh spanning-tree
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.6433.5ED8
             Cost        12
             Port        28(Port-channel3)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0060.7036.96AE
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Po2              Desg FWD 3         128.27   Shr
Po3              Root FWD 12        128.28   Shr

Switch2#
```
## switch3
```
Switch3#sh etherchannel summary
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port

Number of channel-groups in use: 2
Number of aggregators:           2

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           PAgP   Fa0/23(P) Fa0/24(P) 
3      Po3(SU)           LACP   Fa0/10(P) Fa0/11(P) 
Switch3#sh spanning-tree
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.6433.5ED8
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0001.6433.5ED8
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Po3              Desg FWD 12        128.28   Shr
Po1              Desg FWD 12        128.27   Shr

Switch3#
```
Guide: <https://www.youtube.com/watch?v=-ce0RGws2vQ>

