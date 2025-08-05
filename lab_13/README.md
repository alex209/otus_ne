# Лабораторная №13

## Виртуальная частные сети - VPN

### Цели задания

- Настроить GRE между офисами Москва и С.-Петербург
- Настроить DMVPN между офисами Москва и Чокурдах, Лабытнанги

### Топология сети

![](./img/lab_13.png)

### Задачи

- Настроите GRE между офисами Москва и С.-Петербург.
- Настроите DMVMN между Москва и Чокурдах, Лабытнанги.
- Все узлы в офисах в лабораторной работе должны иметь IP связность.
- План работы и изменения зафиксированы в документации.

# Настройка устройств:

Базовая настройка и настройка IP адресов была произведена в предыдущих лабораторных работах.

<details>

<summary><H3>Настройка GRE между офисами Москва и С.-Петербург</H3></summary>

Между офисами Москва и С.-Петербург создадим два GRE канала:

- от R14 до R18 на интерфейс **e0/2**
- от R15 до R18 на интерфейс **e0/3**

## Таблица адресов для GRE между офисами Москва и С.-Петербург

| Device | Interface | IP Address  | Subnet Mask     | Default Gateway | Description     |
| ------ | --------- | ----------- | --------------- | --------------- | --------------- |
| R14    | tun0      | 172.16.10.1 | 255.255.255.252 |                 | to_SPB_R18_e0/2 |
| R15    | tun1      | 172.16.10.5 | 255.255.255.252 |                 | to_SPB_R18_e0/3 |
| R18    | tun0      | 172.16.10.2 | 255.255.255.252 |                 | to_MSK_R14      |
|        | tun1      | 172.16.10.6 | 255.255.255.252 |                 | to_MSK_R15      |

### R14

```
interface Tunnel0
 description to_SPB_R18_e0/2
 ip address 172.16.10.1 255.255.255.252
 tunnel source Ethernet0/2
 tunnel destination 67.73.193.2
 tunnel key 1
!
```

### R15

```
interface Tunnel1
 description to_SPB_R18_e0/3
 ip address 172.16.10.5 255.255.255.252
 tunnel source Ethernet0/2
 tunnel destination 64.210.65.2
 tunnel key 2
!
```

### R18

```
!
interface Tunnel0
 description to_MSK_R14
 ip address 172.16.10.2 255.255.255.252
 tunnel source Ethernet0/2
 tunnel destination 207.231.240.2
 tunnel key 1
!
interface Tunnel1
 description to_MSK_R15
 ip address 172.16.10.6 255.255.255.252
 tunnel source Ethernet0/3
 tunnel destination 128.249.190.2
 tunnel key 2
!
```

### Проверка работы GRE туннелей

#### tun0 R14

!["tun0 R14"](./img/tun0_r14.png)

#### tun1 R15

!["tun1 R15"](./img/tun1_r15.png)

#### tun0 R18

!["tun0 R18"](./img/tun0_r18.png)

#### tun1 R18

!["tun1 R18"](./img/tun1_r18.png)

#### пинги с R18 на адреса туннелей R14 и R15

!["пинги с R18 на адреса туннелей R14 и R15"](./img/ping_r18_tun.png)

</details>

<details>

<summary><H3>Настройка DMVMN между Москва и Чокурдах, Лабытнанги</H3></summary>

## Таблица адресов для mGRE

| Device | Interface | IP Address    | Subnet Mask   | Default Gateway |
| ------ | --------- | ------------- | ------------- | --------------- |
| R14    | tun10     | 172.16.100.14 | 255.255.255.0 |                 |
| R15    | tun10     | 172.16.100.55 | 255.255.255.0 |                 |
| R27    | tun10     | 172.16.100.27 | 255.255.255.0 |                 |
| R28    | tun10     | 172.16.100.28 | 255.255.255.0 |                 |

Маршрутизаторам R14 и R15 в офисе Москва назначим роль **HUB**

### R14

```
!
interface Tunnel10
 description DMVPN
 ip address 172.16.100.14 255.255.255.0
 no ip redirects
 ip nhrp map multicast dynamic
 ip nhrp network-id 100
 tunnel source Ethernet0/2
 tunnel mode gre multipoint
 tunnel key 100
!
```

### R15

```
!
interface Tunnel10
 description DMVPN
 ip address 172.16.100.15 255.255.255.0
 no ip redirects
 ip nhrp map multicast dynamic
 ip nhrp network-id 100
 tunnel source Ethernet0/2
 tunnel mode gre multipoint
 tunnel key 100
!
```

Маршрутизаторам R27 и R28 назначим роль **SPOKE** и создадим **nhrp map** на оба **HUB**

### R27

```
!
interface Tunnel10
 ip address 172.16.100.27 255.255.255.0
 no ip redirects
 ip nhrp map 172.16.100.14 207.231.240.2
 ip nhrp map multicast 207.231.240.2
 ip nhrp map 172.16.100.15 128.249.190.2
 ip nhrp map multicast 128.249.190.2
 ip nhrp network-id 100
 ip nhrp nhs 172.16.100.14
 ip nhrp nhs 172.16.100.15
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
 tunnel key 100
!
```

### R28

```
!
interface Tunnel10
 ip address 172.16.100.28 255.255.255.0
 no ip redirects
 ip nhrp map 172.16.100.14 207.231.240.2
 ip nhrp map 172.16.100.15 128.249.190.2
 ip nhrp map multicast 207.231.240.2
 ip nhrp map multicast 128.249.190.2
 ip nhrp network-id 100
 ip nhrp nhs 172.16.100.14
 ip nhrp nhs 172.16.100.15
 tunnel source Ethernet0/1
 tunnel mode gre multipoint
 tunnel key 100
!
```

### Проверка работы mGRE туннелей

#### dmvpn R14

!["dmvpn R14"](./img/dmvpn_r14.png)

#### nhrp R14

!["nhrp R14"](./img/nhrp_r14.png)

#### dmvpn R15

!["dmvpn R15"](./img/dmvpn_r15.png)

#### nhrp R15

!["nhrp R15"](./img/nhrp_r15.png)

#### dmvpn R27

!["dmvpn R27"](./img/dmvpn_r27.png)

#### nhrp R27

!["nhrp R27"](./img/nhrp_r27.png)

#### dmvpn R28

!["dmvpn R28"](./img/dmvpn_r28.png)

#### nhrp R28

!["nhrp R28"](./img/nhrp_r28.png)

#### пинги от R27

!["пинги от R27"](./img/ping_r27.png)

#### пинги от R28

!["пинги от R28"](./img/ping_r28.png)

</details>

<details>

<summary><H3>Настройка IP связности между офисами</H3></summary>

Для обмена префиксами между офисами будем использовать BGP протокол. В офисе Москва между HUB уже настроена iBGP сессия. Для офисов Лабытнанги и Чокурдах номера AS возьмем из приватного диапазона соответственно 65027 и 65028 и настроим eBGP соседство между HUB и SPOKE.

</details>
