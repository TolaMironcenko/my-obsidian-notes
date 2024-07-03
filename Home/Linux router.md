
## /etc/dnsmasq.conf

```txt
interface=ethX
#no-dhcp-interface=ethX
tworkd instead
expand-hosts
domain=foo.bar
dhcp-range=10.0.0.2,10.0.0.255,255.255.255.0,1h
```

```txt
domain-needed
bogus-priv
dhcp-authoritative
no-dhcp-interface=eth0
interface=wlan0
server=/someremote.lan/192.168.1.1
local=/my.lan/
server=8.8.8.8
server=8.8.4.4
expand-hosts
domain=my.lan
dhcp-range=192.168.251.32,192.168.251.90,12h
dhcp-host=AA:BB:CC:DD:EE:FF,SomeName,192.168.251.129,12h
dhcp-host=00:11:22:33:44:55,hp-printer,192.168.251.210,12h
dhcp-option=119,my.lan,someremote.lan
dhcp-option=252,"\n"
dhcp-host=AA:11:BB:22:CC:33,ignore
cname=SomeOtherName.my.lan,elastix
```

```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

## /etc/netctl/extern0-profile

```txt
Description='Public Interface.'
Interface=extern0
Connection=ethernet
IP='dhcp'
```

## /etc/netctl/intern0-profile

```txt
Description='Private Interface'
Interface=intern0
Connection=ethernet
IP='static'
Address=('10.0.0.1/24')
```

```bash
netctl enable extern0-profile
netctl enable intern0-profile
```

## /etc/sysctl.conf

```txt
# Controls IP packet forwarding
net.ipv4.ip_forward = 1
```

# Chatgpt answers

## Wan Lan

Создание маршрутизатора на компьютере с Linux может стать образовательным и полезным проектом, предоставляющим вам больший контроль над вашей сетью. Вот пошаговое руководство, которое поможет вам превратить ваш компьютер Linux в функциональный маршрутизатор:

### Требования

1. Компьютер Linux как минимум с двумя сетевыми интерфейсами (один для глобальной сети и один для локальной сети).
2. Базовое понимание сетевых концепций.
3. Доступ в Интернет для загрузки необходимого программного обеспечения.

### Пошаговое руководство

#### 1. Установите дистрибутив Linux

Убедитесь, что на вашем компьютере Linux установлена ​​чистая установка популярного дистрибутива, такого как Ubuntu, Debian или CentOS.

#### 2. Обновите систему

Откройте терминал, обновите список пакетов вашей системы и обновите все установленные пакеты.

Для систем на базе Debian/Ubuntu:

```bash
sudo apt update
sudo apt upgrade
```

Для систем на базе CentOS/RHEL:

```bash
sudo yum update
```

#### 3. Включите переадресацию IP-адресов.

Чтобы разрешить вашему компьютеру Linux пересылать пакеты между интерфейсами, включите пересылку IP.

Отредактируйте файл конфигурации sysctl:

```bash
sudo nano /etc/sysctl.conf
```

Раскомментируйте или добавьте следующую строку:

```txt
net.ipv4.ip_forward = 1
```

Примените изменения:

```bash
sudo sysctl -p
```

#### 4. Настройте сетевые интерфейсы

Определите свои сетевые интерфейсы, используя:

```bash
ip addr
```

Предполагая `eth0`, что ваш интерфейс WAN (подключен к Интернету) и `eth1`интерфейс LAN (подключен к вашей локальной сети), настройте интерфейсы соответствующим образом.

Отредактируйте файл конфигурации сетевых интерфейсов (пример для Debian/Ubuntu):

```bash
sudo nano /etc/network/interfaces
```

Добавьте следующие конфигурации:

```txt
# WAN Interface
auto eth0
iface eth0 inet dhcp

# LAN Interface
auto eth1
iface eth1 inet static
    address 192.168.1.1
    netmask 255.255.255.0
```

Перезапустите сетевые службы, чтобы применить изменения:

```bash
sudo systemctl restart networking
```

#### 5. Настройте NAT (трансляцию сетевых адресов)

Используйте `iptables`для настройки NAT, чтобы ваши устройства локальной сети могли получать доступ к Интернету через интерфейс WAN.

```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
```

Чтобы эти правила сохранялись при перезагрузках, сохраните их, используя `iptables-save`:

Для Дебиан/Убунту:

```bash
sudo iptables-save > /etc/iptables/rules.v4
```

alpine linux

```bash
doas /etc/init.d/iptables save
doas rc-update add iptables
```

Для CentOS/RHEL вам может потребоваться использовать `iptables-service`или вручную настроить iptables для загрузки правил при запуске.

#### 6. Настройте DHCP-сервер (необязательно).

Если вы хотите, чтобы маршрутизатор Linux назначал IP-адреса устройствам в вашей локальной сети, установите и настройте DHCP-сервер.

Установить `isc-dhcp-server`:

```bash
sudo apt install isc-dhcp-server
```

Отредактируйте файл конфигурации DHCP-сервера:

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Добавьте конфигурацию подсети:

```txt
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.10 192.168.1.100;
    option routers 192.168.1.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
}
```

Укажите интерфейс, который должен прослушивать DHCP-сервер:

```bash
sudo nano /etc/default/isc-dhcp-server
```

Набор:

```txt
INTERFACESv4="eth1"
```

Запустите и включите DHCP-сервер:

```bash
sudo systemctl start isc-dhcp-server
sudo systemctl enable isc-dhcp-server
```

#### 7. Проверьте настройку.

- Подключите устройство к интерфейсу локальной сети (eth1) и проверьте, получает ли оно IP-адрес (если настроен DHCP).
- Проверьте подключение к Интернету с подключенного устройства.

#### 8. Дополнительная конфигурация (необязательно)

- **Правила брандмауэра:** настройте дополнительные `iptables`правила для повышения безопасности.
- **DNS-сервер:** настройте локальный DNS-сервер для лучшего разрешения DNS в вашей сети.
- **Мониторинг и ведение журнала:** установите такие инструменты, как `tcpdump`, `iftop`или `ntopng`для мониторинга сетевого трафика.

### Заключение

После этих шагов у вас должен получиться функциональный маршрутизатор, построенный на вашем компьютере с Linux. Эта настройка обеспечивает хорошую основу, но вы можете дополнительно настроить и улучшить ее в соответствии с вашими конкретными потребностями.

## wifi router

Создание Wi-Fi-маршрутизатора на компьютере с Linux включает в себя несколько дополнительных шагов по сравнению с проводным маршрутизатором. Вот подробное руководство, которое поможет вам в этом:

### Требования

1. Компьютер Linux с адаптером Wi-Fi, поддерживающим режим AP (точки доступа).
2. По крайней мере один интерфейс Ethernet для подключения WAN.
3. Базовое понимание сетевых концепций.
4. Доступ в Интернет для загрузки необходимого программного обеспечения.

### Пошаговое руководство

#### 1. Установите дистрибутив Linux

Убедитесь, что на вашем компьютере Linux установлена ​​чистая установка популярного дистрибутива, такого как Ubuntu, Debian или CentOS.

#### 2. Обновите систему

Откройте терминал, обновите список пакетов вашей системы и обновите все установленные пакеты.

Для систем на базе Debian/Ubuntu:

```bash
sudo apt update
sudo apt upgrade
```

Для систем на базе CentOS/RHEL:

```bash
sudo yum update
```

#### 3. Включите переадресацию IP-адресов.

Чтобы разрешить вашему компьютеру Linux пересылать пакеты между интерфейсами, включите пересылку IP.

Отредактируйте файл конфигурации sysctl:

```bash
sudo nano /etc/sysctl.conf
```

Раскомментируйте или добавьте следующую строку:

```bash
net.ipv4.ip_forward = 1
```

Примените изменения:

```bash
sudo sysctl -p
```

#### 4. Установите Hostapd и DNSMasq.

Hostapd будет использоваться для создания точки доступа Wi-Fi, а DNSMasq будет управлять службами DHCP и DNS.

Для Дебиан/Убунту:

```bash
sudo apt install hostapd dnsmasq
```

Для CentOS/RHEL:

```bash
sudo yum install hostapd dnsmasq
```

#### 5. Настройте сетевые интерфейсы

Определите свои сетевые интерфейсы, используя:

```bash
ip addr
```

Предполагая `eth0`, что ваш интерфейс WAN (подключен к Интернету) и `wlan0`интерфейс WiFi, настройте интерфейсы соответствующим образом.

Отредактируйте файл конфигурации сетевых интерфейсов (пример для Debian/Ubuntu):

```bash
sudo nano /etc/network/interfaces
```

Добавьте следующие конфигурации:

```txt
# WAN Interface
auto eth0
iface eth0 inet dhcp

# WiFi Interface
auto wlan0
iface wlan0 inet static
    address 192.168.1.1
    netmask 255.255.255.0
```

Перезапустите сетевые службы, чтобы применить изменения:

```bash
sudo systemctl restart networking
```

#### 6. Настройте Hostapd

Отредактируйте файл конфигурации Hostapd:

```bash
sudo nano /etc/hostapd/hostapd.conf
```

Добавьте следующую конфигурацию (настройте по мере необходимости):

```txt
interface=wlan0
ssid=YourSSID
hw_mode=g
channel=6
wpa=2
wpa_passphrase=YourPassphrase
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

Отредактируйте файл конфигурации по умолчанию, чтобы он указывал на вашу пользовательскую конфигурацию:

```bash
sudo nano /etc/default/hostapd
```

Набор:

```txt
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

#### 7. Настройте DNSMasq

Отредактируйте файл конфигурации DNSMasq:

```bash
sudo nano /etc/dnsmasq.conf
```

Добавьте следующие конфигурации:

```txt
interface=wlan0
dhcp-range=192.168.1.2,192.168.1.100,12h
```

#### 8. Настройте NAT (трансляцию сетевых адресов)

Используйте `iptables`для настройки NAT, чтобы ваши устройства Wi-Fi могли получать доступ к Интернету через интерфейс WAN.

```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT
```

Чтобы эти правила сохранялись при перезагрузках, сохраните их, используя `iptables-save`:

Для Дебиан/Убунту:

```bash
sudo iptables-save > /etc/iptables/rules.v4
```

Для CentOS/RHEL вам может потребоваться использовать `iptables-service`или вручную настроить iptables для загрузки правил при запуске.

#### 9. Запустите и включите службы.

Запустите и включите Hostapd и DNSMasq для запуска при загрузке.

```bash
sudo systemctl start hostapd
sudo systemctl start dnsmasq
sudo systemctl enable hostapd
sudo systemctl enable dnsmasq
```

#### 10. Проверьте настройку.

- Подключите устройство к сети Wi-Fi (SSID: `YourSSID`).
- Проверьте, получает ли устройство IP-адрес из диапазона DHCP.
- Проверьте подключение к Интернету с подключенного устройства.

### Заключение

После этих шагов у вас должен получиться работающий Wi-Fi-маршрутизатор, построенный на вашем компьютере с Linux. Эта настройка обеспечивает хорошую основу, но вы можете дополнительно настроить и улучшить ее в соответствии с вашими конкретными потребностями, например, добавив правила брандмауэра, улучшив конфигурации безопасности и оптимизировав производительность.


# Настройка маршрутизатора Linux с `eth0`, `eth1`, и `wlan0`включает в себя настройку сетевых интерфейсов, таблиц маршрутизации и, возможно, правил брандмауэра. Вот базовый план шагов для достижения этого:

### Шаг 1: Установка необходимых пакетов

Убедитесь, что у вас установлены необходимые пакеты. В системе на базе Debian (например, Ubuntu) вы можете установить их с помощью:

```bash
sudo apt-get update
sudo apt-get install isc-dhcp-server iptables
```

### Шаг 2: Настройка сетевых интерфейсов

Отредактируйте файл сетевых интерфейсов. В системах на базе Debian этот файл обычно находится в `/etc/network/interfaces`.

```bash
sudo nano /etc/network/interfaces
```

Настройте интерфейсы по мере необходимости. Вот пример конфигурации:

```bash
# Loopback interface
auto lo
iface lo inet loopback

# eth0 - connected to the internet
auto eth0
iface eth0 inet dhcp

# eth1 - internal network
auto eth1
iface eth1 inet static
  address 192.168.1.1
  netmask 255.255.255.0

# wlan0 - internal wireless network
auto wlan0
iface wlan0 inet static
  address 192.168.2.1
  netmask 255.255.255.0
```

### Шаг 4: Настройка iptables

Настройте iptables для пересылки трафика из внутренней сети во внешнюю сеть.

```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT

```