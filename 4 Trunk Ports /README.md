TRUNK PORTS

Allow devices attached to different switches but assigned to the same vlan group to communicate with one another across the switched topology
Permita que los dispositivos conectados a diferentes conmutadores pero asignados al mismo grupo vlan se comuniquen entre sí a través de la topología conmutada
No son miembros de alguna vlan
Dirigie el trafico de un switch a switch por todas las vlans

Crear un trunk puerto

Para que un Puerto sea trunk se requiere conectarse a otro switch, es switch a switch
Switch> ena
Switch# show vlan brief (muestra las vlans en los puertos)
Switch# config t
Switch (config)# interface fa0/24  (aqui elegimos el rango de puertosdonde se va a asignar la vlan, como best practice se elige el ultimo puerto)
Switch (config-if)# switchport mode trunk (para eligir la opcion trunk en el puerto 24
Switch (config-if)# “ctrl+c”
Switch# show interface trunk (muestra todas las configuracion trunk en el switch
Esta configuración se debe de hacer en los dos switches en los dos puertos conectados

