## Отчет по лабораторной работе №2 "Эмуляция распределенной корпоративной сети связи, настройка статической маршрутизации между филиалами"

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)

Year: 2022/2023

Group: K33202

Author: Osipova Valeriya Vladimirovna

Lab: Lab2

Date of create: 10.12.2022

Date of finished: 15.12.2022

### Цель работы
Ознакомиться с принципами планирования IP адресов, настройке статической маршрутизации и сетевыми функциями устройств.

### Ход работы
#### 1. В файте топологии сети ```lab2.clab.yaml``` пропишем ноды у устройств.
Содержимое файла: 
```
name: lab2

mgmt:
  network: LeraNet
  ipv4_subnet: 192.10.10.0/24

topology:
  nodes:
    R01.MSK:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.10.10.2
    
    R01.FRT:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.10.10.3

    R01.BRL:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.10.10.4


    PC1: 
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.10.10.11

    PC2: 
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.10.10.12

    PC3: 
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.10.10.13

  links:
    - endpoints: ["R01.MSK:eth2", "R01.BRL:eth2"]
    - endpoints: ["R01.BRL:eth3", "R01.FRT:eth2"]
    - endpoints: ["R01.MSK:eth3", "R01.FRT:eth3"]
    - endpoints: ["R01.MSK:eth4", "PC1:eth2"]
    - endpoints: ["R01.FRT:eth4", "PC2:eth2"]
    - endpoints: ["R01.BRL:eth4", "PC3:eth2"]
```

#### 2. Разворачиваем контейнер: 
![image](https://user-images.githubusercontent.com/64967406/207663055-5ca41518-f135-47f5-b83e-dd6c8bb1a8df.png)

#### 3. Прописываем параметры каждому устройству
Текст конфигураций сетевых устройств: 
- Роутер R01.MSK: \
![image](https://user-images.githubusercontent.com/64967406/207665333-30d27e36-522a-4606-ba1c-d3b3596f45c4.png)
- Роутер R01.FRT: \
![image](https://user-images.githubusercontent.com/64967406/207665369-f2a3ef74-6ee2-4631-a8cf-025d23360b46.png)
- Роутер R01.BRL: \
![image](https://user-images.githubusercontent.com/64967406/207665405-e595505b-9c3a-4d94-a1cf-a2645c879738.png)
- PC1: \
![image](https://user-images.githubusercontent.com/64967406/207665497-f6463b64-4f82-4f5a-beeb-400bfe3965d6.png)
- PC2: \
![image](https://user-images.githubusercontent.com/64967406/207665586-9a891050-711b-4035-9b7a-a432e8e350c1.png)
- PC3: \
![image](https://user-images.githubusercontent.com/64967406/207665682-d1bbf98b-609d-4112-97da-3989ea87c45c.png)

#### 4. Схема сети

![image](./lab2-scheme.jpg)

#### 5. Проврека локальной связанности
![image](https://user-images.githubusercontent.com/64967406/207947202-ce4c4a9e-5253-4286-8755-c36d13c3f43c.png)
![image](https://user-images.githubusercontent.com/64967406/207947847-76792b1b-c401-4037-848f-af9987b28544.png)

![image](https://user-images.githubusercontent.com/64967406/207949208-1fdd6a08-c305-497d-8763-cdd5887d7651.png)

#### 6. Вывод
В процессе денной лабораторной работы удалось ознакомиться с принципами планирования IP адресов, настройки статической маршрутизации и сетевыми функциями устройств. Была построена сеть связи в трех геораспределенных офисах "RogaIKopita Games".
