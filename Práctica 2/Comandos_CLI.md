# Comandos CLI utilizados
## Red de la Ciudad Comercial Cayalá

Las PCs se configuraron por interfaz grafica: IP 192.168.[VLAN].11 a .13 con mascara 255.255.255.0 y gateway 192.168.[VLAN].1

**Orden de aplicacion:**
1) hostname en los 6 switches
2) EtherChannel y troncales en los 6 switches
3) VTP y VLANs, un switch a la vez en este orden: Nucleo-Cayala, Z1, Z2, Z3, Z4, Z5. 
4) puertos de acceso, blackhole, Rapid-PVST+ y SVIs
Al ejecutar copy running-config startup-config, presionar Enter para aceptar el nombre del archivo.

> **Nota:** Antes de crear la VLAN del siguiente switch, comprobar con show vlan brief que ya muestre las anteriores.

### Nucleo-Cayala (Cisco 3560-24PS)
```
enable
configure terminal
hostname Nucleo-Cayala
interface range gigabitEthernet 0/1-2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode desirable
exit
interface range fastEthernet 0/1-2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 2 mode desirable
exit
interface fastEthernet 0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 25,99
exit
interface fastEthernet 0/4
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 35,99
exit
interface fastEthernet 0/5
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 55,99
exit
interface port-channel 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 15,55,99
exit
interface port-channel 2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 45,99
exit
vtp mode server
vtp domain 202402955
vtp password Cayala2026
vlan 99
 name NATIVA
exit
vlan 999
 name BLACKHOLE
exit
interface range fastEthernet 0/6-24
 switchport mode access
 switchport access vlan 999
exit
spanning-tree mode rapid-pvst
spanning-tree vlan 99 priority 4096
ip routing
interface vlan 15
 ip address 192.168.15.1 255.255.255.0
 no shutdown
exit
interface vlan 25
 ip address 192.168.25.1 255.255.255.0
 no shutdown
exit
interface vlan 35
 ip address 192.168.35.1 255.255.255.0
 no shutdown
exit
interface vlan 45
 ip address 192.168.45.1 255.255.255.0
 no shutdown
exit
interface vlan 55
 ip address 192.168.55.1 255.255.255.0
 no shutdown
end
copy running-config startup-config
```

### Z1-Comercial (Cisco 2960-24TT)
```
enable
configure terminal
hostname Z1-Comercial
interface range gigabitEthernet 0/1-2
 switchport mode trunk
 channel-group 1 mode desirable
exit
interface port-channel 1
 switchport trunk native vlan 99
 switchport trunk allowed vlan 15,55,99
exit
interface fastEthernet 0/5
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 55,99
exit
vtp mode server
vtp domain 202402955
vtp password Cayala2026
vlan 15
 name COMERCIAL
exit
interface range fa0/11-13
 switchport mode access
 switchport access vlan 15
exit
interface range fa0/1-4, fa0/6-10, fa0/14-24
 switchport mode access
 switchport access vlan 999
exit
spanning-tree mode rapid-pvst
spanning-tree vlan 15 priority 4096
end
copy running-config startup-config
```

### Z2-Residencial (Cisco 2960-24TT)
```
enable
configure terminal
hostname Z2-Residencial
interface fastEthernet 0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 25,99
exit
vtp mode server
vtp domain 202402955
vtp password Cayala2026
vlan 25
 name RESIDENCIAL
exit
interface range fa0/11-13
 switchport mode access
 switchport access vlan 25
exit
interface range fa0/2-10, fa0/14-24, gi0/1-2
 switchport mode access
 switchport access vlan 999
exit
spanning-tree mode rapid-pvst
spanning-tree vlan 25 priority 4096
end
copy running-config startup-config
```

### Z3-Entretenimiento (Cisco 2960-24TT)
```
enable
configure terminal
hostname Z3-Entretenimiento
interface fastEthernet 0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 35,99
exit
vtp mode server
vtp domain 202402955
vtp password Cayala2026
vlan 35
 name ENTRETENIMIENTO
exit
interface range fa0/11-13
 switchport mode access
 switchport access vlan 35
exit
interface range fa0/2-10, fa0/14-24, gi0/1-2
 switchport mode access
 switchport access vlan 999
exit
spanning-tree mode rapid-pvst
spanning-tree vlan 35 priority 4096
end
copy running-config startup-config
```

### Z4-Corporativo (Cisco 2960-24TT)
```
enable
configure terminal
hostname Z4-Corporativo
interface range fastEthernet 0/1-2
 switchport mode trunk
 channel-group 2 mode desirable
exit
interface port-channel 2
 switchport trunk native vlan 99
 switchport trunk allowed vlan 45,99
exit
vtp mode server
vtp domain 202402955
vtp password Cayala2026
vlan 45
 name CORPORATIVO
exit
interface range fa0/11-13
 switchport mode access
 switchport access vlan 45
exit
interface range fa0/3-10, fa0/14-24, gi0/1-2
 switchport mode access
 switchport access vlan 999
exit
spanning-tree mode rapid-pvst
spanning-tree vlan 45 priority 4096
end
copy running-config startup-config
```

### Z5-Seguridad (Cisco 2960-24TT)
```
enable
configure terminal
hostname Z5-Seguridad
interface range fastEthernet 0/1-2
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 55,99
exit
vtp mode server
vtp domain 202402955
vtp password Cayala2026
vlan 55
 name SEGURIDAD
exit
interface range fa0/11-13
 switchport mode access
 switchport access vlan 55
exit
interface range fa0/3-10, fa0/14-24, gi0/1-2
 switchport mode access
 switchport access vlan 999
exit
spanning-tree mode rapid-pvst
spanning-tree vlan 55 priority 4096
end
copy running-config startup-config
```