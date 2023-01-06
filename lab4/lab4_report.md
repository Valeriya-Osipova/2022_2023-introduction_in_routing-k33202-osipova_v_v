## Лабораторная работа №4 "Эмуляция распределенной корпоративной сети связи, настройка iBGP, организация L3VPN, VPLS"

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)

Year: 2022/2023

Group: K33202

Author: Osipova Valeriya Vladimirovna

Lab: Lab4

Date of create: хх.12.2022

Date of finished: хх.12.2022

### Цель работы
Изучить протоколы BGP, MPLS и правила организации L3VPN и VPLS.

### Ход работы
#### 1. В файте топологии сети ```lab4.clab.yaml``` пропишем ноды у устройств.
```
name: lab4

mgmt:
  network: statics
  ipv4_subnet: 172.20.23.0/24

topology:
  nodes:
    R01.SPB:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.23.11

    R01.HKI:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.23.12

    R01.LBN:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.23.13

    R01.LND:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.23.14
    
    R01.NY:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.23.15

    R01.SVL:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.23.16
    
    PC1:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.23.21

    PC2:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.23.22
    
    PC3:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.23.23
    
  links:
    - endpoints: ["R01.SPB:eth1","PC1:eth1"]
    - endpoints: ["R01.SPB:eth2","R01.HKI:eth1"]
    - endpoints: ["R01.HKI:eth2","R01.LND:eth1"]
    - endpoints: ["R01.HKI:eth3","R01.LBN:eth1"]
    - endpoints: ["R01.LBN:eth2","R01.LND:eth2"]
    - endpoints: ["R01.LND:eth3","R01.NY:eth1"]
    - endpoints: ["R01.NY:eth2","PC2:eth1"]
    - endpoints: ["R01.LBN:eth3","R01.SVL:eth1"]
    - endpoints: ["R01.SVL:eth2","PC3:eth1"]
```
#### 2. Разворачиваем контейнер 
![image](https://user-images.githubusercontent.com/64967406/211038808-661d53c3-11d0-49b1-b46b-2820da270568.png)

## Первая часть:

Настраимаем iBGP RR Cluster.
Настраимаем VRF на 3 роутерах.
Настраимаем RD и RT на 3 роутерах.
Настраимаем IP адреса в VRF.
Проверяем связность между VRF
Настраимаем имена устройств, сменить логины и пароли.

#### 1. Прописываем параметры каждому устройству
Текст конфигураций сетевых устройств:
- Роутер R01.SPB
```
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default router-id=1.1.1.1
/routing ospf instance
set [ find default=yes ] router-id=1.1.1.1
/ip address
add address=192.168.10.1/24 interface=ether2 network=192.168.10.0
add address=10.0.1.1/30 interface=ether3 network=10.0.1.0
add address=1.1.1.1 interface=Lo0 network=1.1.1.1
/ip dhcp-client
add disabled=no interface=ether1
add disabled=no interface=ether2
add disabled=no interface=ether3
/ip route vrf
add export-route-targets=65530:777 import-route-targets=65530:777 interfaces=\
    ether2 route-distinguisher=65530:777 routing-mark=VRF_DEVOPS
/mpls ldp
set enabled=yes transport-address=1.1.1.1
/mpls ldp interface
add interface=ether2
add interface=ether3
/routing bgp instance vrf
add redistribute-connected=yes routing-mark=VRF_DEVOPS
/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=\
    2.2.2.2 remote-as=65530 update-source=Lo0
/routing ospf network
add area=backbone
/system identity
set name=R01.SPB
```
- Роутер R01.HKI:

- Роутер R01.LBN:

- Роутер R01.LND:
    
- Роутер R01.NY:

- Роутер R01.SVL:
