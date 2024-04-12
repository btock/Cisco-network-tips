# ETHERCHANNEL
***
## 1- Basic switch configuration (switch 1 & switch 2)
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
## SWITCH 3
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

'''
### Switch 3
'''
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
   * Active: establece que el puerto envíe paquetes para iniciar la negociación del EtherChannel.

## Switch 1
```
