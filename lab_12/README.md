# Лабораторная №12

## Основные протоколы сети интернет

### Цели задания

- Настроить DHCP в офисе Москва
- Настроить синхронизацию времени в офисе Москва
- Настроить NAT в офисе Москва, C.-Перетбруг и Чокурдах

### Топология сети

![](./img/lab_12.png)

### Задачи

- Настроите NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.
- Настроите NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.
- Настроите статический NAT для R20.
- Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.
- Настроите статический NAT(PAT) для офиса Чокурдах.
- Настроите для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP.
- Настроите NTP сервер на R12 и R13. Все устройства в офисе Москва должны синхронизировать время с R12 и R13.
- Все офисы в лабораторной работе должны иметь IP связность.
- План работы и изменения зафиксированы в документации.

# Настройка устройств:

Базовая настройка и настройка IP адресов была произведена в предыдущих лабораторных работах.

<details>

<summary><H3>Настроите NAT(PAT) на R14 и R15 в офисе Москва</H3></summary>

### Создаем списки доступа кому разрешен выход во внешние сети

#### R14 и R15

```
access-list 101 permit ip 192.168.10.0 0.0.0.255 any
access-list 101 permit ip 192.168.20.0 0.0.0.255 any
access-list 101 permit ip 10.100.100.0 0.0.0.255 any
access-list 101 permit ip 10.10.90.0 0.0.0.255 any
```

### Определяем внутренние и внешние интерфейсы

#### R14

```
interface Ethernet0/0
 description to_R12
 ip address 10.10.90.2 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet0/1
 description to_R13
 ip address 10.10.90.9 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet0/2
 description to_R22_AS101
 ip address 207.231.240.2 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/3
 description to_R19
 ip address 10.10.90.34 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet1/0
 description to_R15
 ip address 10.10.90.41 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
```

#### R15

```
interface Ethernet0/0
 description to_R13
 ip address 10.10.90.6 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet0/1
 description to_R12
 ip address 10.10.90.14 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet0/2
 description to_R21_AS301
 ip address 128.249.190.2 255.255.255.248
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/3
 description to_R20
 ip address 10.10.90.38 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet1/0
 description to_R14
 ip address 10.10.90.42 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
```

### Создаем динамическую трансляцию между внутренним локальным и внешним глобальным адресами

#### R14 и R15

```
ip nat inside source list 101 interface Ethernet0/2 overload
```

</details>

<details>

<summary><H3>Настройка для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13</H3></summary>

#### R12

```
!
ip dhcp excluded-address 192.168.10.1
ip dhcp excluded-address 192.168.20.1
!
ip dhcp pool CLIENT10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
!
ip dhcp pool CLIENT20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
!
```

#### R13

```
!
ip dhcp excluded-address 192.168.10.1
ip dhcp excluded-address 192.168.20.1
ip dhcp excluded-address 192.168.10.2 192.168.10.127
ip dhcp excluded-address 192.168.20.2 192.168.20.127
!
ip dhcp pool CLIENT10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
!
ip dhcp pool CLIENT20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
!

```

На коммутаторах SW4 и SW5 настраиваем SVI интерфейсы и VRRP на этих интерфейсах, а также указываем helper адрес через который происходит пересылка широковещательного пакета от клиента одноадресатным пакетом DHCP-серверу.

#### SW4

```
interface Vlan10
 description VLAN 10
 ip address 192.168.10.4 255.255.255.0
 ip helper-address 10.100.100.12
 ip helper-address 10.100.100.13
 ipv6 enable
 ospfv3 1 ipv4 area 10
 vrrp 10 description VLAN10
 vrrp 10 ip 192.168.10.1
 vrrp 10 priority 110
!
interface Vlan20
 description VLAN20
 ip address 192.168.20.4 255.255.255.0
 ip helper-address 10.100.100.12
 ip helper-address 10.100.100.13
 ipv6 enable
 ospfv3 1 ipv4 area 10
 vrrp 20 description VLAN20
 vrrp 20 ip 192.168.20.1
 vrrp 20 priority 110
!
interface Vlan100
 description MGMT
 ip address 10.100.100.204 255.255.255.192
 ipv6 enable
 ospfv3 1 ipv4 area 10
 vrrp 100 description MGMT
 vrrp 100 ip 10.100.100.193
 vrrp 100 priority 110
!
```

#### SW4

```

```

</details>
