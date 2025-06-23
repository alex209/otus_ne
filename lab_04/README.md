# Лабораторная №4

##

### Цели задания

Распланировать адресное пространство, настроить IP на всех активных портах. Адресное пространство должно быть задокументировано.

### Топология сети

![](./img/lab_4_topology.png)

### Задачи:

- Разработаете и задокументируете адресное пространство для лабораторного стенда.
- Настроите ip адреса на каждом активном порту
- Настроите каждый VPC в каждом офисе в своем VLAN.
- Настроите VLAN/Loopbackup interface управления для сетевых устройств
- Настроите сети офисов так, чтобы не возникало broadcast штормов, а использование линков было максимально оптимизировано
- Используете IPv4. IPv6 по желанию

## Таблица адресов для AS 101 (Киторн)

| Device | Interface | IP Address    | Subnet Mask     | Default Gateway | Description   |
| ------ | --------- | ------------- | --------------- | --------------- | ------------- |
| R22    | lo0       | 10.10.1.22    | 255.255.255.255 |                 | Loopback_R22  |
|        | e0/0      | 207.231.240.1 | 255.255.255.252 |                 | to_R14_AS1001 |
|        | e0/1      | 209.124.176.1 | 255.255.255.252 |                 | to_R21_AS301  |
|        | e0/2      | 207.231.242.5 | 255.255.255.252 |                 | to_R23_AS520  |

## Таблица адресов для AS 301 (Ламас)

| Device | Interface | IP Address    | Subnet Mask     | Default Gateway | Description   |
| ------ | --------- | ------------- | --------------- | --------------- | ------------- |
| R21    | lo0       | 10.30.1.21    | 255.255.255.255 |                 | Loopback_R21  |
|        | e0/0      | 128.249.190.1 | 255.255.255.252 |                 | to_R15_AS1001 |
|        | e0/1      | 209.124.176.2 | 255.255.255.252 |                 | to_R22_AS101  |
|        | e0/2      | 128.249.165.1 | 255.255.255.252 |                 | to_R24_AS520  |

## Таблица адресов для AS 520 (Триада)

| Device | Interface | IP Address    | Subnet Mask     | Default Gateway | Description   |
| ------ | --------- | ------------- | --------------- | --------------- | ------------- |
| R23    | lo0       | 10.52.0.23    | 255.255.255.255 |                 | Loopback_R23  |
|        | e0/0      | 207.231.242.6 | 255.255.255.252 |                 | to_R22_AS101  |
|        | e0/1      | 10.30.90.1    | 255.255.255.252 |                 | to_R25        |
|        | e0/2      | 10.30.90.5    | 255.255.255.252 |                 | to_R24        |
| R24    | lo0       | 10.52.0.24    | 255.255.255.255 |                 | Loopback_R24  |
|        | e0/0      | 128.249.165.2 | 255.255.255.252 |                 | to_R21_AS301  |
|        | e0/1      | 10.30.90.13   | 255.255.255.252 |                 | to_R26        |
|        | e0/2      | 10.30.90.6    | 255.255.255.252 |                 | to_R23        |
|        | e0/3      | 67.73.193.1   | 255.255.255.252 |                 | to_R18_AS2042 |
| R25    | lo0       | 10.52.0.25    | 255.255.255.255 |                 | Loopback_R25  |
|        | e0/0      | 10.30.90.2    | 255.255.255.252 |                 | to_R23        |
|        | e0/1      | 67.73.196.5   | 255.255.255.252 |                 | to_R27_ext    |
|        | e0/2      | 10.30.90.9    | 255.255.255.252 |                 | to_R26        |
|        | e0/3      | 67.73.196.1   | 255.255.255.252 |                 | to_R28_ext    |
| R26    | lo0       | 10.52.0.26    | 255.255.255.255 |                 | Loopback_R26  |
|        | e0/0      | 10.30.90.14   | 255.255.255.252 |                 | to_R24        |
|        | e0/1      | 8.242.244.1   | 255.255.255.252 |                 | to_R28_ext    |
|        | e0/2      | 10.30.90.10   | 255.255.255.252 |                 | to_R25        |
|        | e0/3      | 64.210.65.1   | 255.255.255.252 |                 | to_R18_AS2042 |

## Таблица адресов для Лабытнанги

| Device | Interface | IP Address    | Subnet Mask     | Default Gateway | Description  |
| ------ | --------- | ------------- | --------------- | --------------- | ------------ |
| R27    | lo0       | 10.200.200.27 | 255.255.255.255 |                 | Loopback_R27 |
|        | e0/0      | 67.73.196.6   | 255.255.255.252 |                 | to_R25_ext   |

## Таблица адресов AS 1001 (Москва)

| Device | Interface | IP Address     | Subnet Mask     | Default Gateway | Description  |
| ------ | --------- | -------------- | --------------- | --------------- | ------------ |
| R14    | lo0       | 10.100.100.14  | 255.255.255.255 |                 | Loopback_R14 |
|        | e0/0      | 10.10.90.2     | 255.255.255.252 |                 | to_R12       |
|        | e0/1      | 10.10.90.9     | 255.255.255.252 |                 | to_R13       |
|        | e0/2      | 207.231.240.2  | 255.255.255.252 |                 | to_R22_AS101 |
|        | e0/3      | 10.10.90.34    | 255.255.255.252 |                 | to_R19       |
| R15    | lo0       | 10.100.100.15  | 255.255.255.255 |                 | Loopback_R15 |
|        | e0/0      | 10.10.90.6     | 255.255.255.252 |                 | to_R13       |
|        | e0/1      | 10.10.90.14    | 255.255.255.252 |                 | to_R12       |
|        | e0/2      | 128.249.190.2  | 255.255.255.252 |                 | to_R21_AS301 |
|        | e0/3      | 10.10.90.38    | 255.255.255.252 |                 | to_R20       |
| R12    | lo0       | 10.100.100.12  | 255.255.255.255 |                 | Loopback_R12 |
|        | e0/0      | 10.10.90.18    | 255.255.255.252 |                 | to_SW4       |
|        | e0/1      | 10.10.90.29    | 255.255.255.252 |                 | to_SW5       |
|        | e0/2      | 10.10.90.1     | 255.255.255.252 |                 | to_R14       |
|        | e0/3      | 10.10.90.13    | 255.255.255.252 |                 | to_R15       |
| R13    | lo0       | 10.100.100.13  | 255.255.255.255 |                 | Loopback_R13 |
|        | e0/0      | 10.10.90.22    | 255.255.255.252 |                 | to_SW5       |
|        | e0/1      | 10.10.90.26    | 255.255.255.252 |                 | to_SW4       |
|        | e0/2      | 10.10.90.5     | 255.255.255.252 |                 | to_R15       |
|        | e0/3      | 10.10.90.10    | 255.255.255.252 |                 | to_R14       |
| R19    | lo0       | 10.100.100.19  | 255.255.255.255 |                 | Loopback_R19 |
|        | e0/0      | 10.10.90.33    | 255.255.255.252 |                 | to_R14       |
| R20    | lo0       | 10.100.100.20  | 255.255.255.255 |                 | Loopback_R20 |
|        | e0/0      | 10.10.90.37    | 255.255.255.252 |                 | to_R15       |
| SW2    | vlan100   | 10.100.100.202 | 255.255.255.192 |                 | MGMT         |
| SW3    | vlan100   | 10.100.100.203 | 255.255.255.192 |                 | MGMT         |
| SW4    | vlan100   | 10.100.100.204 | 255.255.255.192 |                 | MGMT         |
|        | e1/0      | 10.10.90.17    | 255.255.255.252 |                 | to_R12       |
|        | e1/1      | 10.10.90.25    | 255.255.255.252 |                 | to_R13       |
| SW5    | vlan100   | 10.100.100.205 | 255.255.255.192 |                 | MGMT         |
|        | e1/0      | 10.10.90.21    | 255.255.255.252 |                 | to_R13       |
|        | e1/1      | 10.10.90.30    | 255.255.255.252 |                 | to_R12       |
| VPC1   | eth0      | 192.168.10.10  | 255.255.255.0   | 192.168.10.1    |              |
| VPC7   | eth0      | 192.168.20.10  | 255.255.255.0   | 192.168.20.1    |              |

## Таблица VLAN для AS 1001 (Москва)

| VLAN | Name      | Назначенный интерфейс     |
| ---- | --------- | ------------------------- |
| 10   | Client 10 | SW3: e0/2                 |
| 20   | Client 20 | SW2: e0/2                 |
| 100  | MGMT      | SW2,SW3,SW4,SW5: VLAN 100 |

## Таблица адресов AS 2042 (С.-Петербург)

| Device | Interface | IP Address     | Subnet Mask     | Default Gateway | Description  |
| ------ | --------- | -------------- | --------------- | --------------- | ------------ |
| R16    | lo0       | 10.200.100.16  | 255.255.255.255 |                 | Loopback_R16 |
|        | e0/0      | 10.20.90.5     | 255.255.255.252 |                 | to_SW10      |
|        | e0/1      | 10.20.90.22    | 255.255.255.252 |                 | to_R18       |
|        | e0/2      | 10.20.90.9     | 255.255.255.252 |                 | to_SW9       |
|        | e0/3      | 10.20.90.25    | 255.255.255.252 |                 | to_R32       |
| R17    | lo0       | 10.200.100.17  | 255.255.255.255 |                 | Loopback_R17 |
|        | e0/0      | 10.20.90.1     | 255.255.255.252 |                 | to_SW9       |
|        | e0/1      | 10.20.90.17    | 255.255.255.252 |                 | to_R18       |
|        | e0/2      | 10.20.90.13    | 255.255.255.252 |                 | to_SW10      |
| R18    | lo0       | 10.200.100.18  | 255.255.255.255 |                 | Loopback_R18 |
|        | e0/0      | 10.20.90.21    | 255.255.255.252 |                 | to_R16       |
|        | e0/1      | 10.20.90.18    | 255.255.255.252 |                 | to_R17       |
|        | e0/2      | 67.73.193.2    | 255.255.255.252 |                 | to_R24_AS520 |
|        | e0/3      | 64.210.65.2    | 255.255.255.252 |                 | to_R26_As520 |
| R32    | lo0       | 10.200.100.32  | 255.255.255.255 |                 | Loopback_R32 |
|        | e0/0      | 10.20.90.26    | 255.255.255.252 |                 | to_R16       |
| SW9    | vlan101   | 10.200.100.209 | 255.255.255.192 |                 | MGMT         |
|        | e0/3      | 10.20.90.2     | 255.255.255.252 |                 | to_R17       |
|        | e1/0      | 10.20.90.10    | 255.255.255.252 |                 | to_R16       |
| SW10   | vlan101   | 10.200.100.210 | 255.255.255.192 |                 | MGMT         |
|        | e0/3      | 10.20.90.6     | 255.255.255.252 |                 | to_R16       |
|        | e1/0      | 10.20.90.14    | 255.255.255.252 |                 | to_R17       |
| VPC8   | eth0      | 192.168.11.10  | 255.255.255.0   | 192.168.11.1    |              |
| VPC    | eth0      | 192.168.21.10  | 255.255.255.0   | 192.168.21.1    |              |

## Таблица VLAN для AS 2042 (С.-Петербург)

| VLAN | Name      | Назначенный интерфейс |
| ---- | --------- | --------------------- |
| 11   | Client 11 | SW9: e0/2             |
| 21   | Client 12 | SW10: e0/2            |
| 101  | MGMT      | SW9,SW10: VLAN 101    |

## Таблица адресов Чокурдан

| Device | Interface | IP Address     | Subnet Mask     | Default Gateway | Description  |
| ------ | --------- | -------------- | --------------- | --------------- | ------------ |
| R28    | lo0       | 10.200.200.28  | 255.255.255.255 |                 | Loopback_R28 |
|        | e0/0      | 67.73.196.2    | 255.255.255.252 |                 | to_R26_AS520 |
|        | e0/1      | 8.242.244.2    | 255.255.255.252 |                 | to_R25_AS520 |
|        | e0/2.12   | 192.168.12.1   | 255.255.255.0   |                 |              |
|        | e0/2.22   | 192.168.22.1   | 255.255.255.0   |                 |              |
|        | e0/2.102  | 10.200.200.193 | 255.255.255.192 |                 | MGMT         |
| SW29   | vlan102   | 10.200.200.229 | 255.255.255.192 | 10.200.200.193  | MGMT         |
| VPC30  | eth0      | 192.168.12.10  | 255.255.255.0   | 192.168.12.1    |              |
| VPC31  | eht0      | 192.168.22.10  | 255.255.255.0   | 192.168.22.1    |              |

## Таблица VLAN для Чокурдан

| VLAN | Name      | Назначенный интерфейс |
| ---- | --------- | --------------------- |
| 12   | Client 12 | SW29: e0/0            |
| 22   | Client 22 | SW29: e0/1            |
| 102  | MGMT      | SW29: VLAN 102        |

# Настройка оборудования

<details>
<summary> Настройка базовых параметров </summary>

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

# Настраиваем интерфейсы и ip адреса

## AS 101 (Киторн)

<details>

<summary> Настраиваем интерфейсы для маршрутизатора R22: </summary>

```
interface Loopback0
 description Loopback_R22
 ip address 10.10.1.22 255.255.255.255
!
interface Ethernet0/0
 description to_R14_AS100
 ip address 207.231.240.1 255.255.255.252
!
interface Ethernet0/1
 description to_R21_AS301
 ip address 209.124.176.1 255.255.255.252
!
interface Ethernet0/2
 description to_R23_AS520
 ip address 207.231.242.5 255.255.255.252
!
interface Ethernet0/3
 no ip address
 shutdown
!
interface Ethernet1/0
 no ip address
 shutdown
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown

```

</details>

## AS 301 (Ламас)

<details>
<summary> Настраиваем интерфейсы для маршрутизатора R21: </summary>

```
interface Loopback0
 description Loopback_R21
 ip address 10.30.1.21 255.255.255.255
!
interface Ethernet0/0
 description to_R15_AS1001
 ip address 128.249.190.1 255.255.255.252
!
interface Ethernet0/1
 description to_R22_AS101
 ip address 209.124.176.2 255.255.255.252
!
interface Ethernet0/2
 description to_R24_AS520
 ip address 128.249.165.1 255.255.255.252
!
interface Ethernet0/3
 no ip address
 shutdown
!
interface Ethernet1/0
 no ip address
 shutdown
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown

```

</details>

## AS 520 (Триада)

<details>
<summary> Настраиваем интерфейсы для маршрутизатора R23: </summary>

```
interface Loopback0
 description Loopback_R23
 ip address 10.52.0.23 255.255.255.255
!
interface Ethernet0/0
 description to_R22_AS101
 ip address 207.231.242.6 255.255.255.252
!
interface Ethernet0/1
 description to_R25
 ip address 10.30.90.1 255.255.255.252
!
interface Ethernet0/2
 description to_R24
 ip address 10.30.90.5 255.255.255.252
!
interface Ethernet0/3
 no ip address
 shutdown
!
interface Ethernet1/0
 no ip address
 shutdown
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown

```

</details>

<details>
<summary> Настраиваем интерфейсы для маршрутизатора R24: </summary>

```
interface Loopback0
 description Loopback_R24
 ip address 10.52.0.24 255.255.255.255
!
interface Ethernet0/0
 description to_R21_AS301
 ip address 128.249.165.2 255.255.255.252
!
interface Ethernet0/1
 description to_R26
 ip address 10.30.90.13 255.255.255.252
!
interface Ethernet0/2
 description to_R23
 ip address 10.30.90.6 255.255.255.252
!
interface Ethernet0/3
 description to_R18_AS2
 ip address 67.73.193.1 255.255.255.252
!
interface Ethernet1/0
 no ip address
 shutdown
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown

```

</details>

<details>
<summary> Настраиваем интерфейсы для маршрутизатора R25: </summary>

```
interface Loopback0
 description Loopback_R25
 ip address 10.52.0.25 255.255.255.255
!
interface Ethernet0/0
 description to_R23
 ip address 10.30.90.2 255.255.255.252
!
interface Ethernet0/1
 description to_R27_ext
 ip address 67.73.196.5 255.255.255.252
!
interface Ethernet0/2
 description to_R26
 ip address 10.30.90.9 255.255.255.252
!
interface Ethernet0/3
 description to_R28_ext
 ip address 67.73.196.1 255.255.255.252
!
interface Ethernet1/0
 no ip address
 shutdown
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown

```

</details>

<details>
<summary> Настраиваем интерфейсы для маршрутизатора R26: </summary>

```
interface Loopback0
 description Loopback_R26
 ip address 10.52.0.26 255.255.255.255
!
interface Ethernet0/0
 description to_R24
 ip address 10.30.90.14 255.255.255.252
!
interface Ethernet0/1
 description to_R28_ext
 ip address 8.242.244.1 255.255.255.252
!
interface Ethernet0/2
 description to_R25
 ip address 10.30.90.10 255.255.255.252
!
interface Ethernet0/3
 description to_R18_AS2042
 ip address 64.210.65.1 255.255.255.252
!
interface Ethernet1/0
 no ip address
 shutdown
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown

```

</details>

## Лабытнанги

<details>

<summary> Настраиваем интерфейсы для маршрутизатора R27: </summary>

```
interface Loopback0
 description Loopback_R27
 ip address 10.200.200.27 255.255.255.255
!
interface Ethernet0/0
 description to_R25_ext
 ip address 67.73.196.6 255.255.255.252
!
interface Ethernet0/1
 no ip address
 shutdown
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
interface Ethernet1/0
 no ip address
 shutdown
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown

```

</details>

## AS 1001 (Москва)

<details>

<summary> Настраиваем интерфейсы для маршрутизатора R12: </summary>

```
interface Loopback0
 no shutdown
 description Loopback_R12
 ip address 10.100.100.12 255.255.255.255
!
interface Ethernet0/0
 no shutdown
 description to_SW4
 ip address 10.10.90.18 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 description to_SW5
 ip address 10.10.90.29 255.255.255.252
!
interface Ethernet0/2
 no shutdown
 description to_R14
 ip address 10.10.90.1 255.255.255.252
!
interface Ethernet0/3
 no shutdown
 description to_R15
 ip address 10.10.90.13 255.255.255.252
!
interface Ethernet1/0
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/1
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/2
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/3
 no shutdown
 no ip address
 shutdown

```

</details>

<details>

<summary> Настраиваем интерфейсы для маршрутизатора R13: </summary>

```
interface Loopback0
 no shutdown
 description Loopback_R13
 ip address 10.100.100.13 255.255.255.255
!
interface Ethernet0/0
 no shutdown
 description to_SW5
 ip address 10.10.90.22 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 description to_SW4
 ip address 10.10.90.26 255.255.255.252
!
interface Ethernet0/2
 no shutdown
 description to_R15
 ip address 10.10.90.5 255.255.255.252
!
interface Ethernet0/3
 no shutdown
 description to_R14
 ip address 10.10.90.10 255.255.255.252
!
interface Ethernet1/0
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/1
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/2
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/3
 no shutdown
 no ip address
 shutdown
```

</details>

<details>

<summary> Настраиваем интерфейсы для маршрутизатора R14: </summary>

```
interface Loopback0
 no shutdown
 description Loopback_R14
 ip address 10.100.100.14 255.255.255.255
!
interface Ethernet0/0
 no shutdown
 description to_R12
 ip address 10.10.90.2 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 description to_R13
 ip address 10.10.90.9 255.255.255.252
!
interface Ethernet0/2
 no shutdown
 description to_R22_AS101
 ip address 207.231.240.2 255.255.255.252
!
interface Ethernet0/3
 no shutdown
 description to_R19
 ip address 10.10.90.34 255.255.255.252
!
```

</details>

<details>

<summary> Настраиваем интерфейсы для маршрутизатора R15: </summary>

```
interface Loopback0
 no shutdown
 description Loopback_R15
 ip address 10.100.100.15 255.255.255.255
!
interface Ethernet0/0
 no shutdown
 description to_R13
 ip address 10.10.90.6 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 description to_R12
 ip address 10.10.90.14 255.255.255.252
!
interface Ethernet0/2
 no shutdown
 description to_R21_AS301
 ip address 128.249.190.2 255.255.255.252
!
interface Ethernet0/3
 no shutdown
 description to_R20
 ip address 10.10.90.38 255.255.255.252
```

</details>

<details>

<summary> Настраиваем интерфейсы для маршрутизатора R19: </summary>

```
interface Loopback0
 no shutdown
 description Loopback_R19
 ip address 10.100.100.19 255.255.255.255
!
interface Ethernet0/0
 no shutdown
 description to_R14
 ip address 10.10.90.33 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 no ip address
 shutdown
!
interface Ethernet0/2
 no shutdown
 no ip address
 shutdown
!
interface Ethernet0/3
 no shutdown
 no ip address
 shutdown
```

</details>

<details>

<summary> Настраиваем интерфейсы для маршрутизатора R20: </summary>

```
interface Loopback0
 no shutdown
 description Loopback_R20
 ip address 10.100.100.20 255.255.255.255
!
interface Ethernet0/0
 no shutdown
 description to_R15
 ip address 10.10.90.37 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 no ip address
 shutdown
!
interface Ethernet0/2
 no shutdown
 no ip address
 shutdown
!
interface Ethernet0/3
 no shutdown
 no ip address
 shutdown
```

</details>

<details>

<summary> Настраиваем интерфейсы для маршрутизатора SW2: </summary>

```
interface Ethernet0/0
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 20
 switchport mode access
!
interface Ethernet0/3
 no shutdown
!
interface Ethernet1/0
 no shutdown
!
interface Ethernet1/1
 no shutdown
!
interface Ethernet1/2
 no shutdown
!
interface Ethernet1/3
 no shutdown
!
interface Vlan100
 no shutdown
 description MGMT
 ip address 10.100.100.202 255.255.255.192
!
```

</details>

<details>

<summary> Настраиваем интерфейсы для маршрутизатора SW3: </summary>

```
interface Ethernet0/0
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/3
 no shutdown
!
interface Ethernet1/0
 no shutdown
!
interface Ethernet1/1
 no shutdown
!
interface Ethernet1/2
 no shutdown
!
interface Ethernet1/3
 no shutdown
!
interface Vlan100
 no shutdown
 description MGMT
 ip address 10.200.100.203 255.255.255.192
```

</details>

<details>

<summary> Настраиваем интерфейсы для маршрутизатора SW4: </summary>

```
interface Port-channel1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/0
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-protocol lacp
 channel-group 1 mode active
!
interface Ethernet0/3
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-protocol lacp
 channel-group 1 mode active
!
interface Ethernet1/0
 no shutdown
 description to_R12
 no switchport
 ip address 10.10.90.17 255.255.255.252
 duplex auto
!
interface Ethernet1/1
 no shutdown
 description to_R13
 no switchport
 ip address 10.10.90.25 255.255.255.252
 duplex auto
!
interface Ethernet1/2
 no shutdown
!
interface Ethernet1/3
 no shutdown
!
interface Vlan100
 no shutdown
 description MGMT
 ip address 10.100.100.204 255.255.255.192
```

</details>

<details>

<summary> Настраиваем интерфейсы для маршрутизатора SW5: </summary>

```
interface Port-channel1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/0
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-protocol lacp
 channel-group 1 mode passive
!
interface Ethernet0/3
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-protocol lacp
 channel-group 1 mode passive
!
interface Ethernet1/0
 no shutdown
 description to_R13
 no switchport
 ip address 10.10.90.21 255.255.255.252
 duplex auto
!
interface Ethernet1/1
 no shutdown
 description to_R12
 no switchport
 ip address 10.10.90.30 255.255.255.252
 duplex auto
!
interface Ethernet1/2
 no shutdown
!
interface Ethernet1/3
 no shutdown
!
interface Vlan100
 no shutdown
 description MGMT
 ip address 10.100.100.205 255.255.255.192
```

</details>
