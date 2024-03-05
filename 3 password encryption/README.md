# Password Encryption
Secret command encrypt the password automatically
R1(config)#username tom secret Cisco





Configurar password para la consola exec mode (outband connection) console 0
Switch>ena
Switch# config t (configure from terminal)
Switch (config)#line console 0
Switch (config-line)# password MyConsolePassword (commando password seguido del password a configurar)
Switch (config-line)# login (commando para decirle al usuario que use password para logearse al router)
Switch (config-line)# exit
Switch (config)# exit
Switch# show run (para ver la configuración en RAM del router y validar si tiene password)
 
Configurar password para in band connection VTY 
Switch>ena
Switch# config t (configure from terminal)
Switch (config)#line VTY 0
Switch (config-line)# password MyVTY0Password (commando password seguido del password a configurar)
Switch (config-line)# login (commando para forzar a que use password para logearse al router)
Switch (config-line)# exit
Switch (config)# exit
Switch# show run (para ver la configuración en RAM del router y validar si tiene password)

 

Poner password a los restantes VTY 1-4
Switch>ena
Switch# config t (configure from terminal)
Switch (config)#line vty 1 4
Switch (config-line)# password MyVTY14Password (commando password seguido del password a configurar)
Switch (config-line)# login (commando para forzar a que use password para logearse al router)
Switch (config-line)# exit
Switch (config)# exit
Switch# show run (para ver la configuración en RAM del router y validar si tiene password)
 
 
Configurar password para la consola privileged exec mode 
Puede elegirse entre encriptarlo y no encriptarlo
Switch>ena
Switch# config t (configure from terminal)
Switch (config)#enable password MyEnablePassword (commando para configurar el password en enable sin encriptar
Switch (config)# enable secret MySecretPassword (commando para encriptar el password)
Switch (config)# exit
Switch# show run
 
Si se configuran los dos password el secret tendrá prioridad ante el password
Encriptar todos los passwords console, vty secret, password:
Switch>ena
Switch# config t (configure from terminal)
Switch (config)# service password-encryption (encrypt all the password in router)
Control c
Switch# show run
 
 
Al hacer estas nuevas configuraciones siempre copiar todo a la NVRAM
Switch# copy running-config startup-config o copy run start
Switch # show startup-config

