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
# 2- 
