https://itexamanswers.net/3-6-2-lab-implement-vlans-and-trunking-answers.html

# Часть 1. Создание сети и настройка основных параметров устройства

## Шаг 1. Настройте базовые параметры каждого коммутатора.

## Шаг 2. Настройте базовые параметры каждого коммутатора.
### a 
Подключитесь к коммутатору с помощью консольного подключения и активируйте
привилегированный режим EXEC.

```
enable
config t
```
### b 
Присвойте коммутатору имя устройства.
```
hostname S1
```
### c 
Отключите поиск DNS.
```
no ip domain-lookup
```
### d
Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
```
enable secret class
```
### e
Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю
```
line console 0
password cisco
login
exit
```
### f
Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
```
line vty 0 15
password cisco
login
exit
```
### g
Зашифруйте открытые пароли.
```
service password-encryption
```
### h
Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
```
banner motd # You must be authorizeded! #
```
### i
Скопируйте текущую конфигурацию в файл загрузочной конфигурации.
```
exit
copy running-config startup-config
```
### S1
```
Switch>enable
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)#enable secret class
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#service password-encryption
S1(config)#banner motd # You must be authorizeded! #
S1(config)#exit
S1#
%SYS-5-CONFIG_I: Configured from console by console

S1#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
S1#
```
### S2
```
Switch>enable
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname S1
S1(config)#hostname S2_Shestakov
S2_Shestakov(config)#no ip domain-lookup
S2_Shestakov(config)#enable secret class
S2_Shestakov(config)#line console 0
S2_Shestakov(config-line)#password cisco
S2_Shestakov(config-line)#login
S2_Shestakov(config-line)#exit
S2_Shestakov(config)#line vty 0 15
S2_Shestakov(config-line)#password cisco
S2_Shestakov(config-line)#login
S2_Shestakov(config-line)#exit
S2_Shestakov(config)#service password-encryption
S2_Shestakov(config)#banner motd # You must be authorizeded! #
S2_Shestakov(config)#exit
S2_Shestakov#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
S2_Shestakov#
```
## Шаг 3.  Настройте узлы ПК.
# Часть 2. . Создание сетей VLAN и назначение портов коммутатора
В части 2 на обоих коммутаторах будут созданы VLAN, как указано в таблице выше. Затем вам нужно
назначить сети VLAN соответствующему интерфейсу. Для проверки параметров конфигурации
используйте команду show vlan. Выполните следующие задачи на каждом коммутаторе.

## Шаг 1. Создайте сети VLAN на коммутаторах.
### a
Создайте необходимые VLAN и назовите их на каждом коммутаторе из приведенной выше
таблицы.

*S1*

```
config t
 
vlan 37
name Management
vlan 47
name Sales
vlan 57
name Operations
vlan 999
name ParkingLot
vlan 1000
name Native
```

*S2*

```
config t
 
vlan 37
name Management
vlan 57
name Operations
vlan 999
name ParkingLot
vlan 1000
name Native
```
### b
Настройте интерфейс управления на каждом коммутаторе, используя информацию об IP-адресе в
таблице адресации.

*S1*

```
config t
interface vlan 37
ip address 192.168.37.11 255.255.255.0
interface vlan 47
ip address 192.168.47.11 255.255.255.0
interface vlan 57
ip address 192.168.57.11 255.255.255.0
```

*S2*

```
S2_Shestakov#config t
Enter configuration commands, one per line.  End with CNTL/Z.
S2_Shestakov(config)#interface vlan 37
S2_Shestakov(config-if)#ip address 192.168.37.11 255.255.255.0
S2_Shestakov(config-if)#interface vlan 57
S2_Shestakov(config-if)#ip address 192.168.57.11 255.255.255.0
```


### c
Назначьте все неиспользуемые порты коммутатора VLAN ParkingLot, настройте их для
статического режима доступа и деактивируйте их административно.

*S1*
```
S1(config-if)#interface range f0/2 - 5, f0/7 - 24, g0/1 - 2
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown
```

*S2*
```
S2_Shestakov(config)#interface range f0/2 - 17, f0/19 - 24, g0/1 - 2
S2_Shestakov(config-if-range)#switchport mode access
S2_Shestakov(config-if-range)#switchport access vlan 999
S2_Shestakov(config-if-range)#shutdown
```
## Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.

### a
Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и
настройте их для режима доступа

*S1*
```
S1(config)#interface f0/6
S1(config-if)#switchport mode access
S1(config-if)#switchport access vlan 47
```
*S2*
```
S2_Shestakov(config)#interface f0/18
S2_Shestakov(config-if)#switchport mode access
S2_Shestakov(config-if)#switchport access vlan 57
```
### b
Убедитесь, что VLAN назначены на правильные интерфейсы.
```
show vlan brief
```
# Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами
В части 3 вручную настраивается интерфейс F0/1 в качестве магистрального канала.

## Шаг 1. Вручную настройте магистральный интерфейс F0/1.

### a
Измените режим порта коммутатора на интерфейсе F0/1, чтобы принудительно создать
магистральную связь. Не забудьте сделать это на обоих коммутаторах.

```
interface f0/1
switchport mode trunk
```
### b
Установите для native VLAN значение 1000 на обоих коммутаторах.
```
switchport trunk native vlan 1000
```
### c
В качестве другой части конфигурации магистрали укажите, что только VLAN X+10, X+20, X+30 и
1000 могут пересекать магистраль.
```
switchport trunk allowed vlan 37,47,57,1000
```
### d
Выполните команду show interfaces trunk для проверки портов магистрали, native VLAN и
разрешенных VLAN через магистраль.
```
show interfaces trunk
```
## Шаг 2. Проверьте подключение.
Проверка подключения во VLAN. Например, PC-A должен успешно выполнить эхо-запрос на S1 во
VLAN X+20.
Вопрос:
Были ли эхо-запросы от PC-B к S2_ФАМИЛИЯ успешными? Дайте пояснение.

**PC-A**
```
ping 192.168.47.11
```
**Были ли эхо-запросы от PC-B к S2_ФАМИЛИЯ успешными? Дайте пояснение**

Соединения не были успешными, поскольку они находятся в разных VLAN.
Для связи между VLAN необходим маршрутизатор.

