## SSH configuration
![ssh](https://github.com/btock/Cisco-network-tips/assets/14008255/6d60f695-21a6-4df5-a301-38f3bae58377)
***
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
***
