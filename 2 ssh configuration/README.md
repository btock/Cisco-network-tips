## SSH configuration

ena
config t

hostname sshconfigtest

ip domain name test.com

crypto key generate rsa

1024

username admin secret test

line vty 0 5

transport input ssh

login local

exit

ip ssh version 2

exit
