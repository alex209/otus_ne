# Лабораторная №9

## BGP

### Цели задания

- Настроить BGP между автономными системами
- Организовать доступность между офисами Москва и С.-Петербург

### Топология сети

![](./img/lab_09.png)

### Задачи

- Настроите eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас.
- Настроите eBGP между провайдерами Киторн и Ламас.
- Настроите eBGP между Ламас и Триада.
- Настроите eBGP между офисом С.-Петербург и провайдером Триада.
- Организуете IP доступность между пограничным роутерами офисами Москва и С.-Петербург.
- План работы и изменения зафиксированы в документации.

## Таблица адресов

Pv4 адреса для оборудования берутся из предыдущей [лабораторной работы #4](../lab_04/README.md)

## AS 101 (Киторн)

### IPv4 адреса

| Device | Interface | IP Address    | Subnet Mask     | Default Gateway | Description   |
| ------ | --------- | ------------- | --------------- | --------------- | ------------- |
| R22    | lo0       | 10.10.1.22    | 255.255.255.255 |                 | Loopback_R22  |
|        | e0/0      | 207.231.240.1 | 255.255.255.252 |                 | to_R14_AS1001 |
|        | e0/1      | 209.124.176.1 | 255.255.255.252 |                 | to_R21_AS301  |
|        | e0/2      | 207.231.242.5 | 255.255.255.252 |                 | to_R23_AS520  |

### IPv6 адреса

| Device | Interface | IPv6 Address             | Default Gateway | Description   |
| ------ | --------- | ------------------------ | --------------- | ------------- |
| R22    | lo0       |                          |                 | Loopback_R22  |
|        | e0/0      | 2001:1860:4000:100::1/64 |                 | to_R14_AS1001 |
|        | e0/1      | 2001:1860:4000:200::1/64 |                 | to_R21_AS301  |
|        | e0/2      | 2001:1860:4000:300::5/64 |                 | to_R23_AS520  |

## AS 301 (Ламас)

### IPv4 адреса

| Device | Interface | IP Address    | Subnet Mask     | Default Gateway | Description   |
| ------ | --------- | ------------- | --------------- | --------------- | ------------- |
| R21    | lo0       | 10.30.1.21    | 255.255.255.255 |                 | Loopback_R21  |
|        | e0/0      | 128.249.190.1 | 255.255.255.252 |                 | to_R15_AS1001 |
|        | e0/1      | 209.124.176.2 | 255.255.255.252 |                 | to_R22_AS101  |
|        | e0/2      | 128.249.165.1 | 255.255.255.252 |                 | to_R24_AS520  |

### IPv6 адреса

| Device | Interface | IPv6 Address             | Default Gateway | Description   |
| ------ | --------- | ------------------------ | --------------- | ------------- |
| R21    | lo0       |                          |                 | Loopback_R21  |
|        | e0/0      | 2001:468:1A08:1001::1/64 |                 | to_R15_AS1001 |
|        | e0/1      | 2001:1860:4000:200::2/64 |                 | to_R22_AS101  |
|        | e0/2      | 2620:0:5070:301::1/64    |                 | to_R24_AS520  |

## AS 520 (Триада)

### IPv4 адреса

| Device | Interface | IP Address    | Subnet Mask     | Default Gateway | Description   |
| ------ | --------- | ------------- | --------------- | --------------- | ------------- |
| R23    | lo0       | 10.52.0.23    | 255.255.255.255 |                 | Loopback_R23  |
|        | e0/0      | 207.231.242.6 | 255.255.255.252 |                 | to_R22_AS101  |
| R24    | lo0       | 10.52.0.24    | 255.255.255.255 |                 | Loopback_R24  |
|        | e0/0      | 128.249.165.2 | 255.255.255.252 |                 | to_R21_AS301  |
|        | e0/3      | 67.73.193.1   | 255.255.255.252 |                 | to_R18_AS2042 |
| R26    | lo0       | 10.52.0.26    | 255.255.255.255 |                 | Loopback_R26  |
|        | e0/3      | 64.210.65.1   | 255.255.255.252 |                 | to_R18_AS2042 |

### IPv6 адреса

| Device | Interface | IPv6 Address             | Default Gateway | Description   |
| ------ | --------- | ------------------------ | --------------- | ------------- |
| R23    | lo0       | 2001:DB8::520:23/128     |                 | Loopback_R23  |
|        | e0/0      | 2001:1860:4000:300::6/64 |                 | to_R22_AS101  |
| R24    | lo0       | 2001:DB8::520:24/128     |                 | Loopback_R24  |
|        | e0/0      | 2620:0:5070:301::2/64    |                 | to_R21_AS301  |
|        | e0/3      | 2C0F:F400:10FF:1::1/64   |                 | to_R18_AS2042 |
| R26    | lo0       | 2001:DB8::520:26/128     |                 | Loopback_R26  |
|        | e0/3      | 2C0F:F400:10FF:2::1/6    |                 | to_R18_AS2042 |

## AS 1001 (Москва)

### IPv4 адреса

| Device | Interface | IP Address    | Subnet Mask     | Default Gateway | Description  |
| ------ | --------- | ------------- | --------------- | --------------- | ------------ |
| R14    | lo0       | 10.100.100.14 | 255.255.255.255 |                 | Loopback_R14 |
|        | e0/2      | 207.231.240.2 | 255.255.255.252 |                 | to_R22_AS101 |
| R15    | lo0       | 10.100.100.15 | 255.255.255.255 |                 | Loopback_R15 |
|        | e0/2      | 128.249.190.2 | 255.255.255.252 |                 | to_R21_AS301 |

### IPv6 адреса

| Device | Interface | IPv6 Address             | Default Gateway | Description  |
| ------ | --------- | ------------------------ | --------------- | ------------ |
| R14    | lo0       | 2001:DB8::1001:14/128    |                 | Loopback_R14 |
|        | e0/2      | 2001:1860:4000:100::2/64 |                 | to_R22_AS101 |
| R15    | lo0       | 2001:DB8::1001:15/128    |                 | Loopback_R15 |
|        | e0/2      | 2001:468:1A08:1001::2/64 |                 | to_R21_AS301 |

## AS 2042 (С.-Петербург)

### IPv4 адреса

| Device | Interface | IP Address    | Subnet Mask     | Default Gateway | Description  |
| ------ | --------- | ------------- | --------------- | --------------- | ------------ |
| R18    | lo0       | 10.200.100.18 | 255.255.255.255 |                 | Loopback_R18 |
|        | e0/2      | 67.73.193.2   | 255.255.255.252 |                 | to_R24_AS520 |
|        | e0/3      | 64.210.65.2   | 255.255.255.252 |                 | to_R26_As520 |

### IPv6 адреса

| Device | Interface | IPv6 Address           | Default Gateway | Description  |
| ------ | --------- | ---------------------- | --------------- | ------------ |
| R18    | lo0       | 2001:DB8::2042:18/128  |                 | Loopback_R23 |
|        | e0/2      | 2C0F:F400:10FF:1::2/64 |                 | to_R24_AS520 |
|        | e0/3      | 2C0F:F400:10FF:2::2/64 |                 | to_R26_AS520 |

# Настройка устройств:

<details>
<summary> Настройка базовых параметров</summary>

Настройка произведена в [лабораторной работе № 4](../lab_04/README.md)

- Присвойте имена устройствам в соответствии с топологией.

```
 (config)# hostname <X><n>
```

    где \<X> R - маршрутизатор S - коммутатор </br>
        \<n> номер устройства

- Отключение поиска DNS

```
 (config)# no ip domain-lookup
```

- Назначьте **class** в качестве зашифрованного пароля доступа к привилегированному режиму.

```
 (config)# enable secret class
```

- Назначьте **cisco** в качестве паролей консоли и VTY

```
 (config)# line console 0
 (config-line)# password cisco
 (config-line)# login
```

```
 (config)# line vty 0 4
 (config-line)# password cisco
 (config-line)# login
```

- Включить шифрование паролей

```
 (config)# service password-encryption
```

- Настройка баннерного сообщения дня (MOTD) для предупреждения пользователей о запрете несанкционированного доступа.

```
 (config)# banner motd "Unauthorized access denied"
```

- Сохранение конфигурации

```
 #copy running-config startup-config
```

</details>

# Настраиваем IPv6 адресов на маршрутизаторах и процесса BGP

<details>

<summary> Настраиваем R22 AS101 (Киторн): </summary>

```
!
interface Loopback0
 description Loopback_R22
 ip address 10.10.1.22 255.255.255.255
!
interface Ethernet0/0
 description to_R14_AS100
 ip address 207.231.240.1 255.255.255.252
 ipv6 address 2001:1860:4000:100::1/64
 ipv6 enable
!
interface Ethernet0/1
 description to_R21_AS301
 ip address 209.124.176.1 255.255.255.252
 ipv6 address 2001:1860:4000:200::1/64
 ipv6 enable
!
interface Ethernet0/2
 description to_R23_AS520
 ip address 207.231.242.5 255.255.255.252
 ipv6 address 2001:1860:4000:300::5/64
 ipv6 enable
!

```

### процесс BGP

```
router bgp 101
 bgp router-id 10.10.1.22
 bgp log-neighbor-changes
 neighbor 2001:1860:4000:100::2 remote-as 1001
 neighbor 2001:1860:4000:200::2 remote-as 301
 neighbor 2001:1860:4000:300::6 remote-as 520
 neighbor 207.231.240.2 remote-as 1001
 neighbor 207.231.242.6 remote-as 520
 neighbor 209.124.176.2 remote-as 301
 !
 address-family ipv4
  network 207.231.240.0 mask 255.255.255.252
  network 207.231.242.4 mask 255.255.255.252
  network 209.124.176.0 mask 255.255.255.252
  no neighbor 2001:1860:4000:100::2 activate
  no neighbor 2001:1860:4000:200::2 activate
  no neighbor 2001:1860:4000:300::6 activate
  neighbor 207.231.240.2 activate
  neighbor 207.231.242.6 activate
  neighbor 209.124.176.2 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:1860:4000:100::/64
  network 2001:1860:4000:200::/64
  network 2001:1860:4000:300::/64
  neighbor 2001:1860:4000:100::2 activate
  neighbor 2001:1860:4000:200::2 activate
  neighbor 2001:1860:4000:300::6 activate
 exit-address-family
!
```

</details>

<details>

<summary> Настраиваем R21 AS301 (Ламас): </summary>

```
!
interface Loopback0
 description Loopback_R21
 ip address 10.30.1.21 255.255.255.255
!
interface Ethernet0/0
 description to_R15_AS1001
 ip address 128.249.190.1 255.255.255.252
 ipv6 address 2001:468:1A08:1001::1/64
 ipv6 enable
!
interface Ethernet0/1
 description to_R22_AS101
 ip address 209.124.176.2 255.255.255.252
 ipv6 address 2001:1860:4000:200::2/64
 ipv6 enable
!
interface Ethernet0/2
 description to_R24_AS520
 ip address 128.249.165.1 255.255.255.252
 ipv6 address 2620:0:5070:301::1/64
 ipv6 enable
!

```

### процесс BGP

```
!
router bgp 301
 bgp router-id 10.30.1.21
 bgp log-neighbor-changes
 neighbor 2001:468:1A08:1001::2 remote-as 1001
 neighbor 2001:1860:4000:200::1 remote-as 101
 neighbor 2620:0:5070:301::2 remote-as 520
 neighbor 128.249.165.2 remote-as 520
 neighbor 128.249.190.2 remote-as 1001
 neighbor 209.124.176.1 remote-as 101
 !
 address-family ipv4
  network 128.249.165.0 mask 255.255.255.252
  network 128.249.190.0 mask 255.255.255.252
  network 209.124.176.0 mask 255.255.255.252
  no neighbor 2001:468:1A08:1001::2 activate
  no neighbor 2001:1860:4000:200::1 activate
  no neighbor 2620:0:5070:301::2 activate
  neighbor 128.249.165.2 activate
  neighbor 128.249.190.2 activate
  neighbor 209.124.176.1 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:468:1A08:1001::/64
  network 2001:1860:4000:200::/64
  network 2620:0:5070:301::/64
  neighbor 2001:468:1A08:1001::2 activate
  neighbor 2001:1860:4000:200::1 activate
  neighbor 2620:0:5070:301::2 activate
 exit-address-family
!

```

</details>

<details>

<summary> Настраиваем R23, R24, R26 AS520 (Триада): </summary>

### R23 IPv6 адреса

```
interface Loopback0
 description Loopback_R23
 ip address 10.52.0.23 255.255.255.255
 ip router isis
 ipv6 address 2001:DB8::520:23/128
 ipv6 enable
 ipv6 router isis
!
interface Ethernet0/0
 description to_R22_AS101
 ip address 207.231.242.6 255.255.255.252
 ipv6 address 2001:1860:4000:300::6/64
 ipv6 enable
!

```

### процесс BGP R23

```
router bgp 520
 bgp router-id 10.52.0.23
 bgp log-neighbor-changes
 neighbor 2001:1860:4000:300::5 remote-as 101
 neighbor 207.231.242.5 remote-as 101
 !
 address-family ipv4
  network 207.231.242.4 mask 255.255.255.252
  no neighbor 2001:1860:4000:300::5 activate
  neighbor 207.231.242.5 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:1860:4000:300::/64
  neighbor 2001:1860:4000:300::5 activate
 exit-address-family
!

```

### R24 IPv6 адреса

```
!
interface Loopback0
 description Loopback_R24
 ip address 10.52.0.24 255.255.255.255
 ip router isis
 ipv6 address 2001:DB8::520:24/128
 ipv6 enable
 ipv6 router isis
!
interface Ethernet0/0
 description to_R21_AS301
 ip address 128.249.165.2 255.255.255.252
 ipv6 address 2620:0:5070:301::2/64
 ipv6 enable
!
interface Ethernet0/3
 description to_R18_AS2024
 ip address 67.73.193.1 255.255.255.252
 ipv6 address 2C0F:F400:10FF:1::1/64
 ipv6 enable
!

```

### процесс BGP R24

```
!
router bgp 520
 bgp router-id 10.52.0.24
 bgp log-neighbor-changes
 neighbor 2620:0:5070:301::1 remote-as 301
 neighbor 2C0F:F400:10FF:1::2 remote-as 2042
 neighbor 67.73.193.2 remote-as 2042
 neighbor 128.249.165.1 remote-as 301
 !
 address-family ipv4
  network 67.73.193.0 mask 255.255.255.252
  network 128.249.165.0 mask 255.255.255.252
  no neighbor 2620:0:5070:301::1 activate
  no neighbor 2C0F:F400:10FF:1::2 activate
  neighbor 67.73.193.2 activate
  neighbor 128.249.165.1 activate
 exit-address-family
 !
 address-family ipv6
  network 2620:0:5070:301::/64
  network 2C0F:F400:10FF:1::/64
  neighbor 2620:0:5070:301::1 activate
  neighbor 2C0F:F400:10FF:1::2 activate
 exit-address-family
!

```

### R26 IPv6 адреса

```
!
interface Loopback0
 description Loopback_R26
 ip address 10.52.0.26 255.255.255.255
 ip router isis
 ipv6 address 2001:DB8::520:26/128
 ipv6 enable
 ipv6 router isis
!
interface Ethernet0/3
 description to_R18_AS2042
 ip address 64.210.65.1 255.255.255.252
 ipv6 address 2C0F:F400:10FF:2::1/64
 ipv6 enable
!

```

### процесс BGP R26

```
!
router bgp 520
 bgp router-id 10.52.0.26
 bgp log-neighbor-changes
 neighbor 2C0F:F400:10FF:2::2 remote-as 2042
 neighbor 64.210.65.2 remote-as 2042
 !
 address-family ipv4
  network 64.210.65.0 mask 255.255.255.252
  no neighbor 2C0F:F400:10FF:2::2 activate
  neighbor 64.210.65.2 activate
 exit-address-family
 !
 address-family ipv6
  network 2C0F:F400:10FF:2::/64
  neighbor 2C0F:F400:10FF:2::2 activate
 exit-address-family
!

```

</details>

<details>

<summary> Настраиваем R14, R15 AS1001 (Москва): </summary>

### R14 IPv6 адреса

```
!
interface Ethernet0/2
 description to_R22_AS101
 ip address 207.231.240.2 255.255.255.252
 ipv6 address 2001:1860:4000:100::2/64
 ipv6 enable

```

### процесс BGP R14

```
!
router bgp 1001
 bgp router-id 10.100.100.14
 bgp log-neighbor-changes
 neighbor 2001:1860:4000:100::1 remote-as 101
 neighbor 207.231.240.1 remote-as 101
 !
 address-family ipv4
  network 207.231.240.0 mask 255.255.255.252
  no neighbor 2001:1860:4000:100::1 activate
  neighbor 207.231.240.1 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:1860:4000:100::/64
  neighbor 2001:1860:4000:100::1 activate
 exit-address-family
!

```

### R15 IPv6 адреса

```
!
interface Ethernet0/2
 description to_R21_AS301
 ip address 128.249.190.2 255.255.255.252
 ipv6 address 2001:468:1A08:1001::2/64
 ipv6 enable

```

### процесс BGP R15

```
!
router bgp 1001
 bgp router-id 10.100.100.15
 bgp log-neighbor-changes
 neighbor 2001:468:1A08:1001::1 remote-as 301
 neighbor 128.249.190.1 remote-as 301
 !
 address-family ipv4
  network 128.249.190.0 mask 255.255.255.252
  no neighbor 2001:468:1A08:1001::1 activate
  neighbor 128.249.190.1 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:468:1A08:1001::/64
  neighbor 2001:468:1A08:1001::1 activate
 exit-address-family
!

```

</details>

<details>

<summary> Настраиваем R18 AS2042 (С.-Петербург): </summary>

### R18 IPv6 адреса

```
!
interface Ethernet0/2
 description to_R24_AS520
 ip address 67.73.193.2 255.255.255.252
 ipv6 address 2C0F:F400:10FF:1::2/64
 ipv6 enable
!
interface Ethernet0/3
 description to_R26_AS520
 ip address 64.210.65.2 255.255.255.252
 ipv6 address 2C0F:F400:10FF:2::2/64
 ipv6 enable
!

```

### процесс BGP R18

```
!
router bgp 2042
 bgp router-id 10.200.100.18
 bgp log-neighbor-changes
 neighbor 2C0F:F400:10FF:1::1 remote-as 520
 neighbor 2C0F:F400:10FF:2::1 remote-as 520
 neighbor 64.210.65.1 remote-as 520
 neighbor 67.73.193.1 remote-as 520
 !
 address-family ipv4
  network 64.210.65.0 mask 255.255.255.252
  network 67.73.193.0 mask 255.255.255.252
  no neighbor 2C0F:F400:10FF:1::1 activate
  no neighbor 2C0F:F400:10FF:2::1 activate
  neighbor 64.210.65.1 activate
  neighbor 67.73.193.1 activate
 exit-address-family
 !
 address-family ipv6
  network 2C0F:F400:10FF:1::/64
  network 2C0F:F400:10FF:2::/64
  neighbor 2C0F:F400:10FF:1::1 activate
  neighbor 2C0F:F400:10FF:2::1 activate
 exit-address-family
!

```

</details>

# Проверка работоспособности

### [Файлы конфигураций устройств ](./config/)
