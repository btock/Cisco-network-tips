en
config t
!
!
hostname BasicSW
!
!
banner motd #this is bto's company switch, kindly Stay out!#
!
!
ip domain-name bto.com
!
crypto key generate rsa
1024
y
!
ip ssh time-out 60
!
ip ssh authentication-retries 2
!
ip ssh ver 2
!
username beto secret ciscotest
!
line vty 0 4
!
transport input ssh 
!
login local
!
exit
!
enable secret ciscotest
username beto secret ciscotest
service password-encryption
!
line con 0
password ciscotest
login
exit
!
exit
!
copy running-config startup-config

!
