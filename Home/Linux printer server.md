
## Ubuntu
Установите необходимые пакеты для сервера печати CUPS и связанные компоненты, вводя следующую команду:

```bash
sudo apt-get install cups avahi-daemon avahi-discover
```

Дополнительно установите Foomatic, который облегчит настройку принтеров в Debian и других операционных системах:

```bash
sudo apt-get install foomatic-db foomatic-db-engine
```

Настройка конфигурационного файла CUPS.

Для разрешения доступа к веб-интерфейсу управления CUPS с любого устройства в локальной сети, выполните следующие шаги:

Откройте конфигурационный файл CUPS в текстовом редакторе nano:

```bash
sudo nano /etc/cups/cupsd.conf
```

Найдите строку "Listen localhost:631" и удалите ее или закомментируйте символом "#" в начале строки.

```txt
# Only listen for connections from local machine.
#Listen localhost:631
Port 631
```

Для открытия доступа к админ-панели CUPS через локальную сеть, выполните следующие изменения в конфигурационном файле:

```txt
# Restrict access to the server...

<Location />

    Order allow,deny

    Allow @Local /// add

</Location>

# Restrict access to the admin pages...

<Location /admin>

    Order allow,deny

    Allow @Local /// add

</Location>

# Restrict access to configuration files...

<Location /admin/conf>

    AuthType Default

    Require user @SYSTEM

    Order allow,deny

    Allow @Local /// add

</Location>
```

Сохраните изменения и закройте файл.

Для сохранения в редакторе nano необходимо нажать Ctrl+x.

Перезапустите службу CUPS, чтобы применить настройки:

```bash
sudo service cups restart
```

or

```bash
reboot -f
```

