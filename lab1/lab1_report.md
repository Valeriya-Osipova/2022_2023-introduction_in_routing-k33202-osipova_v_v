## Отчет по лабораторной работе №1 "Установка ContainerLab и развертывание тестовой сети связи"

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)

Year: 2022/2023

Group: K33202

Author: Osipova Valeriya Vladimirovna

Lab: Lab1

Date of create: 23.09.2022

Date of finished: -

### Цель работы

Ознакомиться с инструментом ContainerLab и методами работы с ним, изучить работу VLAN, IP адресации и т.д.

### Ход работы

#### 1. Текст файла lab.clab.yaml для развертывания сети.
```
name: lab1

mgmt:
  network: statics
  ipv4_subnet: 172.20.20.0/24

topology:
  nodes:
    R01.TEST:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.2

    SW01.L3.01.TEST:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.3

    SW02.L3.01.TEST:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.4

    SW02.L3.02.TEST:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.5

    PC1:
      kind: linux
      image: ubuntu:latest
      mgmt_ipv4: 172.20.20.6

    PC2:
      kind: linux
      image: ubuntu:latest
      mgmt_ipv4: 172.20.20.7

  links: 
    - endpoints: ["R01.TEST:eth1", "SW01.01.TEST:eth1"]
    - endpoints: ["SW01.01.TEST:eth2", "SW02.01.TEST:eth1"]
    - endpoints: ["SW01.01.TEST:eth3", "SW02.02.TEST:eth1"]
    - endpoints: ["SW02.01.TEST:eth2", "PC1:eth1"]
    - endpoints: ["SW02.02.TEST:eth2", "PC2:eth1"]
    
```

#### 2. Разворачиваем сеть с помощью ```clab deploy``` 
![image](https://user-images.githubusercontent.com/64967406/205115297-31237790-bc71-46bd-aafe-7e580acee082.png)

#### 3. Текст конфигураций для кажного сетевого устройства 
- Роутер R01.TEST \
![image](https://user-images.githubusercontent.com/64967406/205116286-23fa603f-361b-4567-a902-7269f43d1944.png)
- Коммутатор SW01.01.TEST \
![image](https://user-images.githubusercontent.com/64967406/205116556-e5019ec7-f2f3-4029-bb5e-cb5bc7f63514.png)
- Коммутатор SW02.01.TEST \
![image](https://user-images.githubusercontent.com/64967406/205116740-fa80fbfd-76a8-4dca-ba66-e5e81417475f.png)
- Коммутатор SW02.02.TEST \
![image](https://user-images.githubusercontent.com/64967406/205116830-6316114c-c3ba-46b8-8a9f-b288d0340e35.png)

#### 4. Проверка пинга
\
![image](https://user-images.githubusercontent.com/64967406/205117615-62dea737-8169-481b-9a5d-6bc79ba45708.png)
![image](https://user-images.githubusercontent.com/64967406/205117662-f2e14983-eada-40d3-a91e-d47b967cdc08.png)

#### 5. Вывод
В ходе лабораторной работы 1 был изучен процесс разворачивания сети, состоящей из роутера, трёх коммутаторов и двух компьютеров. Были настроены VLAN'ы и DHCP сервера.
