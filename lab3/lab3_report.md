## Лабораторная работа №3 "Эмуляция распределенной корпоративной сети связи, настройка OSPF и MPLS, организация первого EoMPLS"

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)

Year: 2022/2023

Group: K33202

Author: Osipova Valeriya Vladimirovna

Lab: Lab3

Date of create: 18.12.2022

Date of finished: ХХ.12.2022

### Цель работы
Изучить протоколы OSPF и MPLS, механизмы организации EoMPLS.

### Ход работы
#### 1. В файте топологии сети ```lab3.clab.yaml``` пропишем ноды у устройств.
Содержимое файла:
```
name: lab3

mgmt:
    network: Net3
    ipv4_subnet: 192.30.31.0/24

topology:
    nodes:
        R01.NY:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 192.30.31.11
        R01.LND:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 192.30.31.12
        R01.LBN:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 192.30.31.13
        R01.HKI:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 192.30.31.14
        R01.MSK:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 192.30.31.15
        R01.SPB:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 192.30.31.16
        SGI_Prism:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 192.30.31.17
        PC1:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 192.30.31.18
    links:
        - endpoints: ["R01.NY:eth1", "SGI_Prism:eth1"]
        - endpoints: ["R01.NY:eth2", "R01.LND:eth1"]
        - endpoints: ["R01.NY:eth3", "R01.LBN:eth1"]
        - endpoints: ["R01.LND:eth2", "R01.HKI:eth1"]
        - endpoints: ["R01.LBN:eth2", "R01.MSK:eth1"]
        - endpoints: ["R01.LBN:eth3", "R01.HKI:eth3"]
        - endpoints: ["R01.HKI:eth2", "R01.SPB:eth2"]
        - endpoints: ["R01.MSK:eth2", "R01.SPB:eth1"]
        - endpoints: ["R01.SPB:eth3", "PC1:eth1"]
```

#### 2. Разворачиваем контейнер 
![image](https://user-images.githubusercontent.com/64967406/208748337-9ba00c2b-332d-432a-98ab-723dd760fb3c.png)

#### 3. Прописываем параметры каждому устройству
Текст конфигураций сетевых устройств:
- Роутер R01.NY

![image](https://user-images.githubusercontent.com/64967406/208748936-21aad112-b731-4bcd-8121-a5c6268510e9.png)

- Роутер R01.LND

![image](https://user-images.githubusercontent.com/64967406/208748995-dd493e65-dedf-419d-91f0-7dbc80ec409f.png)

- Роутер R01.LBN

![image](https://user-images.githubusercontent.com/64967406/208749020-3fec6d71-1a45-4019-88d1-b9d7c6349456.png)

- Роутер R01.HKI

![image](https://user-images.githubusercontent.com/64967406/208749044-a71f5c31-1c7d-4064-8c90-5c2f55ac18e5.png)

- Роутер R01.MSK

![image](https://user-images.githubusercontent.com/64967406/208749078-2a27cb8d-d432-48b1-bcfa-a5262682bb34.png)

- Роутер R01.SPB

![image](https://user-images.githubusercontent.com/64967406/208749111-84f96504-79ff-4503-b0eb-2474697f64f6.png)

- SGI-Prism

![image](https://user-images.githubusercontent.com/64967406/208749132-7581df16-b5ff-43d8-8bcf-30cc45496346.png)

- PC1

![image](https://user-images.githubusercontent.com/64967406/208749158-6d359ba9-cf2f-4eb0-8952-d88f2227fe15.png)

#### 4. Таблицы маршрутизации для каждого роутера:
![image](https://user-images.githubusercontent.com/64967406/208749449-8bced840-a35c-4ea2-8096-89928e60340c.png)
![image](https://user-images.githubusercontent.com/64967406/208749506-73bb199e-c2da-4661-bb5a-1f25963a1580.png)
![image](https://user-images.githubusercontent.com/64967406/208749525-881cad9b-858d-404c-88c1-2237e476de0a.png)
![image](https://user-images.githubusercontent.com/64967406/208749545-477ed878-2cbb-4257-ae27-c28f6705eb31.png)
![image](https://user-images.githubusercontent.com/64967406/208749563-c52f55d2-6957-4e2e-981e-4969c48e1680.png)
![image](https://user-images.githubusercontent.com/64967406/208749589-40185275-2ab2-4619-a629-00b139287de7.png)

#### 5. Информация об MPLS метках:
![image](https://user-images.githubusercontent.com/64967406/208749822-4550cd53-e49c-46a3-9816-8afd21119230.png)

#### 6. Схема сети

#### 7. Проверка пинга ПК и сервера
![image](https://user-images.githubusercontent.com/64967406/208754908-09103fb4-8a9b-4ea3-929f-881134c80155.png)
![image](https://user-images.githubusercontent.com/64967406/208754943-c550ca3b-964f-4499-8f3c-7c893f004ad5.png)

#### 8. Вывод
В результате выполения данной лабораторной работы были изучены протоколы OSPF и MPLS, а также механизмы организации EoMPLS.
