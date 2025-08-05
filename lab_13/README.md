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

Для обмена префиксами между офисами будем использовать BGP протокол. В офисе Москва между HUB уже настроена iBGP сессия. Для офисов Лабытнанги и Чокурдах номера AS возьмем из приватного диапазона, соответственно 65027 и 65028 и настроим eBGP соседство между HUB и SPOKE. Такая конфигурация подразумевает использование DMVPN во 2-й фазе spoke-to-spoke.

На HUB (R14 b R15) настроим BGP соседство со SPOKE через peer-group.

#### R14

```
router bgp 1001
 bgp router-id 10.100.100.14
 bgp log-neighbor-changes
 bgp listen range 172.16.100.0/24 peer-group REM_OFFICE
 neighbor REM_OFFICE peer-group
 neighbor REM_OFFICE remote-as 65027 alternate-as 65028
 neighbor 10.10.90.42 remote-as 1001
 neighbor 2001:DB8:1415::15 remote-as 1001
 neighbor 2001:1860:4000:100::1 remote-as 101
 neighbor 172.16.10.2 remote-as 2042
 neighbor 207.231.240.1 remote-as 101
 !
 address-family ipv4
  network 10.10.90.40 mask 255.255.255.252
  network 207.231.240.0 mask 255.255.255.252
  redistribute ospfv3 1 route-map rm_for_VPN
  neighbor REM_OFFICE activate
  neighbor REM_OFFICE route-map rm_REM_OFFICE out
  neighbor 10.10.90.42 activate
  no neighbor 2001:DB8:1415::15 activate
  no neighbor 2001:1860:4000:100::1 activate
  neighbor 172.16.10.2 activate
  neighbor 172.16.10.2 route-map rm_REM_OFFICE out
  neighbor 207.231.240.1 activate
  neighbor 207.231.240.1 prefix-list pl_RFC_1918 out
  neighbor 207.231.240.1 filter-list 1 out
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:1415::/64
  network 2001:1860:4000:100::/64
  neighbor 2001:DB8:1415::15 activate
  neighbor 2001:1860:4000:100::1 activate
  neighbor 2001:1860:4000:100::1 filter-list 1 out
 exit-address-family
!
```

#### R15

```
router bgp 1001
 bgp router-id 10.100.100.15
 bgp log-neighbor-changes
 bgp listen range 172.16.100.0/24 peer-group REM_OFFICE
 neighbor REM_OFFICE peer-group
 neighbor REM_OFFICE remote-as 65027 alternate-as 65028
 neighbor 10.10.90.41 remote-as 1001
 neighbor 2001:468:1A08:1001::1 remote-as 301
 neighbor 2001:DB8:1415::14 remote-as 1001
 neighbor 128.249.190.1 remote-as 301
 neighbor 172.16.10.6 remote-as 2042
 !
 address-family ipv4
  network 10.10.90.40 mask 255.255.255.252
  network 128.249.190.0 mask 255.255.255.248
  redistribute ospfv3 1 route-map rm_for_VPN
  neighbor REM_OFFICE activate
  neighbor REM_OFFICE route-map rm_REM_OFFICE out
  neighbor 10.10.90.41 activate
  no neighbor 2001:468:1A08:1001::1 activate
  no neighbor 2001:DB8:1415::14 activate
  neighbor 128.249.190.1 activate
  neighbor 128.249.190.1 prefix-list pl_RFC_1918 out
  neighbor 128.249.190.1 route-map rm_ALL_OFFICE in
  neighbor 128.249.190.1 filter-list 1 out
  neighbor 172.16.10.6 activate
  neighbor 172.16.10.6 route-map rm_REM_OFFICE out
 exit-address-family
 !
 address-family ipv6
  network 2001:468:1A08:1001::/64
  network 2001:DB8:1415::/64
  neighbor 2001:468:1A08:1001::1 activate
  neighbor 2001:468:1A08:1001::1 route-map rm_ALL_OFFICE_v6 in
  neighbor 2001:468:1A08:1001::1 filter-list 1 out
  neighbor 2001:DB8:1415::14 activate
 exit-address-family
!
```

Настраиваем BGP протокол на SPOKE и анонсируем необходимые локальные сети

#### R27

```
!
router bgp 65027
 bgp log-neighbor-changes
 network 10.200.200.27 mask 255.255.255.255
 neighbor 172.16.100.14 remote-as 1001
 neighbor 172.16.100.15 remote-as 1001
!
```

#### R28

```
!
router bgp 65028
 bgp log-neighbor-changes
 network 10.200.200.28 mask 255.255.255.255
 network 10.200.200.192 mask 255.255.255.192
 network 192.168.12.0
 network 192.168.22.0
 neighbor 172.16.100.14 remote-as 1001
 neighbor 172.16.100.15 remote-as 1001
!
```

## С.-Петербург

В офисе С.-Петербург тоже настраиваем BGP соседство с Московским офисом.

#### R18

```
router bgp 2042
 bgp router-id 10.200.100.18
 bgp log-neighbor-changes
 bgp listen range 172.16.10.0/29 peer-group pg_VPN_MSK
 bgp bestpath as-path multipath-relax
 neighbor pg_VPN_MSK peer-group
 neighbor pg_VPN_MSK remote-as 1001
 neighbor 2C0F:F400:10FF:1::1 remote-as 520
 neighbor 2C0F:F400:10FF:2::1 remote-as 520
 neighbor 64.210.65.1 remote-as 520
 neighbor 67.73.193.1 remote-as 520
 !
 address-family ipv4
  network 64.210.65.0 mask 255.255.255.248
  network 67.73.193.0 mask 255.255.255.248
  redistribute eigrp 2042 route-map rm_for_VPN
  neighbor pg_VPN_MSK activate
  neighbor pg_VPN_MSK prefix-list for_VPN out
  no neighbor 2C0F:F400:10FF:1::1 activate
  no neighbor 2C0F:F400:10FF:2::1 activate
  neighbor 64.210.65.1 activate
  neighbor 64.210.65.1 prefix-list pl_OUT out
  neighbor 67.73.193.1 activate
  neighbor 67.73.193.1 prefix-list pl_OUT out
  maximum-paths 2
 exit-address-family
 !
 address-family ipv6
  maximum-paths 2
  network 2C0F:F400:10FF:1::/64
  network 2C0F:F400:10FF:2::/64
  neighbor 2C0F:F400:10FF:1::1 activate
  neighbor 2C0F:F400:10FF:1::1 prefix-list pl_OUT_v6 out
  neighbor 2C0F:F400:10FF:2::1 activate
  neighbor 2C0F:F400:10FF:2::1 prefix-list pl_OUT_v6 out
 exit-address-family
!
```

с помощью редистрибуции и префикс листов необходимые перфиксы передадим в Московский офис.

```
!
ip prefix-list for_VPN seq 10 permit 192.168.11.0/24
ip prefix-list for_VPN seq 20 permit 192.168.21.0/24
ip prefix-list for_VPN seq 30 permit 10.200.100.0/24 le 32
!
route-map rm_for_VPN permit 10
 match ip address prefix-list for_VPN
!
```

## Москва

#### R14 & R15

С помощью prefix-list и route-map необходимые перфиксы передадим на все Удаленные офисы.

```
!
ip prefix-list for_VPN seq 10 permit 192.168.10.0/24
ip prefix-list for_VPN seq 20 permit 192.168.20.0/24
ip prefix-list for_VPN seq 30 permit 10.100.100.0/24 le 32
!
ip prefix-list pl_REM_OFFICE seq 5 permit 192.168.12.0/24
ip prefix-list pl_REM_OFFICE seq 20 permit 192.168.22.0/24
ip prefix-list pl_REM_OFFICE seq 30 permit 10.200.200.0/24 le 32
!
ip prefix-list pl_SBP seq 10 permit 192.168.11.0/24
ip prefix-list pl_SBP seq 20 permit 192.168.21.0/24
ip prefix-list pl_SBP seq 30 permit 10.200.100.0/24 le 32
!
!
route-map rm_REM_OFFICE permit 10
 match ip address prefix-list pl_REM_OFFICE for_VPN pl_SBP
!

```

</details>
