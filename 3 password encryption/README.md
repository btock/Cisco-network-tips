# Password Encryption
***
```
"Secret" is a command to encrypt the password automatically
Switch(config)#username beto secret ciscotest
Switch(config)#username beto2 password ciscotest
```


![pass encryption](https://github.com/btock/Cisco-network-tips/assets/14008255/35749b28-be13-4887-b6f5-4fa674c1695b)




## Configurar password para la consola exec mode (outband connection) console 0
```
Switch>ena
Switch# config t (configure from terminal)
Switch (config)#line console 0
Switch (config-line)# password cisctotest (commando password seguido del password a configurar)
Switch (config-line)# login (commando para decirle al usuario que use password para logearse al router)
Switch (config-line)# exit
Switch (config)# exit
Switch# show run (para ver la configuración en RAM del router y validar si tiene password)
```
 ![console password](https://github.com/btock/Cisco-network-tips/assets/14008255/be0bae41-d8ca-4690-8b62-55c04708e151)

 
## Configurar password para in band connection VTY
```
Switch>ena
Switch# config t (configure from terminal)
Switch (config)#line VTY 0 5
Switch (config-line)# password ciscotest (commando password seguido del password a configurar)
Switch (config-line)# login (commando para forzar a que use password para logearse al router)
Switch (config-line)# exit
Switch (config)# exit
Switch# show run (para ver la configuración en RAM del router y validar si tiene password)
```
![vty password](https://github.com/btock/Cisco-network-tips/assets/14008255/5e37bbe9-61a7-4138-a6d6-5ec4211e46b2)

1. VTY es la puerta de enlace para la administración remota de dispositivos de red a través de una conexión de red, generalmente utilizando protocolos como Telnet o SSH (Secure Shell).
2. Permite a los administradores iniciar sesión en el dispositivo y realizar tareas de configuración, monitoreo y solución de problemas sin estar físicamente presentes en el lugar donde está ubicado el dispositivo.
 
## Configurar password para la consola privileged exec mode 
Puede elegirse entre encriptarlo y no encriptarlo
```
Switch>ena
Switch# config t (configure from terminal)
Switch (config)#enable password MyEnablePassword (commando para configurar el password en enable sin encriptar
Switch (config)# enable secret MySecretPassword (commando para encriptar el password)
Switch (config)# exit
Switch# show run
```
 
**Si se configuran los dos password el secret tendrá prioridad ante el password**

![privileged password](https://github.com/btock/Cisco-network-tips/assets/14008255/f48238d0-5b76-4843-9359-455bf23d8fbc)

## Encriptar todos los passwords console, vty secret, password:
```
Switch>ena
Switch# config t (configure from terminal)
Switch (config)# service password-encryption (encrypt all the password in router)
Control c
Switch# show run
```
![encryption password](https://github.com/btock/Cisco-network-tips/assets/14008255/6ae60489-f43d-4049-b145-85b02393dafd)

## Remove passwords
 
 
Al hacer estas nuevas configuraciones siempre copiar todo a la NVRAM
```
Switch# copy running-config startup-config o copy run start
Switch # show startup-config
```

