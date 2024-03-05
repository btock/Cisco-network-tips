## SSH configuration
![ssh](https://github.com/btock/Cisco-network-tips/assets/14008255/6d60f695-21a6-4df5-a301-38f3bae58377)
***

Hay cuatro pasos obligatorios para habilitar el soporte de SSH en un router del Cisco IOS:

1. Configure el comando hostname.

2. Configure el dominio DNS.

3. Genere la clave SSH.

4. Habilite la compatibilidad con el transporte SSH para el vtys.

Si desea que un dispositivo actúe como cliente de SSH al otro, puede agregar SSH a un segundo dispositivo llamado Reed. Esto pone a estos dispositivos en una configuración de cliente y servidor, en la que Carter actúa como servidor y Reed como cliente. La configuración de cliente de SSH del Cisco IOS en Reed es la misma que la requerida para la configuración de servidor de SSH en Carter.

```
ena
config t
hostname sshconfigtest
ip domain name test.com
crypto key generate rsa
1024
username admin secret test
username admin2 secret ciscotest
line vty 0 5
transport input ssh
login local
exit
ip ssh version 2
exit
```
***
