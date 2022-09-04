# UNIX/Linux operating systems



## Contents

1 [Инструмент ipcalc](#part-1-ipcalc-tool)  
2 [Статическая маршрутизация между двумя машинами](#part-2-static-routing-between-two-machines)  
3 [Утилита ipref3](#part-3-ipref3-utility)   
4 [Сетевой экран](#part-4-network-firewall)  
5 [Статическая маршрутизация сети](#part-5-static-network-routing)  
6 [Динмаическая настройка IP с помощью DHCP](#part-6-dynamic-ip-configuration-using-DHCP)  
7 [NAT](#part-7-NAT)  
8 [Дополнтельною Знакомство с SSH Tunnels](#part-8-bonus-introduction-to-ssh-tunnels)  



## Part 1. Ipcalc tool 

### 1.1 Сети и маски

![ipcalc_1.1](screens/ipcalc_1.1.png)

1) Адресс сети 192.167.38.54/13

![ipcalc_1_2](screens/ipcalc_1_2.png)

2) Перевод маски 255.255.255.0 в префиксную и двоичную запись, /15 в обычную и двоичную, 11111111.11111111.11111111.11110000 в обычную и префиксную

![ipcalc_1_3](screens/ipcalc_1_3.png)

![ipcalc_1_3](screens/ipcalc_1_4.png)

3) Минимальный и максимальный хост в сети 12.167.38.4 при масках: /8, 11111111.11111111.00000000.00000000, 255.255.254.0 и /4

### 1.2 Localhost

Определить и записать в отчёт, можно ли обратиться к приложению, работающему на localhost, со следующими IP: 194.34.23.100, 127.0.0.2, 127.1.0.1, 128.0.0.1

![localhost_1](screens/localhost_1.png)

в таком случае нельзя обратиться к приложению, работающему на localhost с IP 194.34.23.100

![localhost_2](screens/localhost_2.png)

а также к приложению, работающему на localhost с IP 128.0.0.1

### 1.3. Диапазоны и сегменты сетей

Определить и записать в отчёт:

1) какие из перечисленных IP можно использовать в качестве публичного, а какие только в качестве частных: 10.0.0.45, 134.43.0.2, 192.168.4.2, 172.20.250.4, 172.0.2.1, 192.172.0.1, 172.68.0.2, 172.16.255.255, 10.10.10.10, 192.169.168.1

PRIVATE:

![private_1](screens/private_1.png)

![private_2](screens/private_2.png)

![private_3](screens/private_3.png)

![private_4](screens/private_4.png)

![private_5](screens/private_5.png)

PUBLIC:

![public_1](screens/public_1.png)

![public_2](screens/public_2.png)

![public_3](screens/public_3.png)

![public_4](screens/public_4.png)

![public_5](screens/public_5.png)

2) какие из перечисленных IP адресов шлюза возможны у сети 10.10.0.0/18: 10.0.0.1, 10.10.0.2, 10.10.10.10, 10.10.100.1, 10.10.1.255

![ip_shluz](screens/ip_shluz.png)

10.0.0.1 -

10.10.0.2 +

10.10.10.10 +

10.10.100.1 -

10.10.1.255 +

## Part 2. Static routing between two machines

С помощью команды ip a посмотреть существующие сетевые интерфейсы

![ip_a_ws1](screens/ip_a_ws1.png)

net interfaces ws1

![ip_a_ws2](screens/ip_a_ws2.png)

net interfaces ws2

Описать сетевой интерфейс, соответствующий внутренней сети, на обеих машинах и задать следующие адреса и маски: ws1 - 192.168.100.10, маска /16, ws2 - 172.24.116.8, маска /12

![static_ws1](screens/static_ws1.png)

etc/netplan/00-installer-config.yaml для ws1

![static_ws2](screens/static_ws2.png)

etc/netplan/00-installer-config.yaml для ws2

Выполнить команду netplan apply для перезапуска сервиса сети

![netplan_ws1](screens/netplan_ws1.png)

![netplan_ws2](screens/netplan_ws2.png)

Добавить статический маршрут от одной машины до другой и обратно при помощи команды вида ip r add

![route_ws1](screens/route_ws1.png)

![route_ws1](screens/route_ws1.png)

Пропинговать соединение между машинами

![ping_ws1](screens/ping_ws1.png)

![ping_ws2](screens/ping_ws2.png)

Перезапустить машины 

Добавить статический маршрут от одной машины до другой с помощью файла etc/netplan/00-installer-config.yaml

для ws1

![static_ws1](screens/static_ws1.png)

для ws2

![static_ws2](screens/static_ws2.png)

Пропинговать соединение между машинами

Пинг с ws1 до ws2

![ping_ws1_2](screens/ping_ws1_2.png)

Пинг с ws2 до ws1

![ping_ws2_2](screens/ping_ws2_2.png)

## Part 3. Iperf3 utility

3.1 Скорость соединения
    
    Перевести и записать в отчет

3.2 Утилита iperf3

Измерить скорость соединения между ws1 и ws2

Для измерения скорости запустим ws2 как сервер, клиентом будет выступать ws1

![ws2_server](screens/ws2_server.png)

![ws1_client](screens/ws1_client.png)

И аналогично обратная ситуация ws1 - сервер ws2 - клиент

![ws1_server](screens/ws1_server.png)

![ws2_client](screens/ws2_client.png)

## Part 4. Network firewall

4.1 Утилита iptables

Создать файл /etc/firewall.sh, имитирующий фаерволл, на ws1 и ws2:

ws1:

![ws1_firewall](screens/ws1_firewall.png)

ws2:

![ws2_firewall](screens/ws2_firewall.png)

Запустить файлы на обеих машинах командами chmod +x /etc/firewall.sh и /etc/firewall.sh

ws1:

![ws1_chmod](screens/ws1_chmod.png)

ws2:

![ws2_chmod](screens/ws2_chmod.png)

4.2. Утилита nmap


Командой ping найти машину, которая не "пингуется"

ws1 to ws2:

![ws1_pingfire](screens/ws1_pingfire.png)

ws2 to ws1:

![ws2_pingfire](screens/ws2_pingfire.png)

Утилитой nmap показать, что хост машины запущен:

![nmap](screens/nmap.png)

## Part 5. Static network routing

Поднять пять виртуальных машин (3 рабочие станции (ws11, ws21, ws22) и 2 роутера (r1, r2))

5.1. Настройка адресов машин

Настроить конфигурации машин в etc/netplan/00-installer-config.yaml

ws11:

![ws11_netplan](screens/ws11_netplan.png)

r1:

![r1_netplan](screens/r1_netplan.png)

r2:

![r2_netplan](screens/r2_netplan.png)

ws21:

![ws21_netplan](screens/ws21_netplan.png)

ws22:

![ws22_netplan](screens/ws22_netplan.png)

Перезапустить сервис сети. Если ошибок нет, то командой ip -4 a проверить, что адрес машины задан верно. Также пропинговать ws22 с ws21. Аналогично пропинговать r1 с ws11.

ws21:

![ws22_ping](screens/ws22_ping.png)

ws22:

![ws22_ipa](screens/ws22_ipa.png.png)

r2:

![r2_ipa](screens/r2_ipa.png.png)

r1:

![r1_ipa](screens/r1_ipa.png)

ws11:

![ws11_ipa](screens/ws11_ipa.png)

5.2. Включение переадресации IP-адресов.

Для включения переадресации IP, выполните команду на роутерах:

    sudo sysctl -w net.ipv4.ip_forward=1

r1:

![r1_sysctl](screens/r1_sysctl.png)

r2:

![r2_sysctl](screens/r2_sysctl.png)

Откройте файл /etc/sysctl.conf и добавьте в него следующую строку:

    net.ipv4.ip_forward = 1

r1:

![r1_config](screens/r1_config.png)

r2:

![r2_config](screens/r2_config.png)

Настроить маршрут по-умолчанию (шлюз) для рабочих станций. Для этого добавить gateway4 [ip роутера] в файле конфигураций

Вызвать ip r и показать, что добавился маршрут в таблицу маршрутизации

ws11:

![ws11_gateway](screens/ws11_gateway.png)

![ws11_route](screens/ws11_route.png)


## Part 6. Dynamic IP configuration using DHCP

    $sudo apt install ntp

Если вам необходимо только синхронизировать время, то утилиты _timesyncd_ вполне хватает для этой простой задачи. Но иногда вам нужен более широкий функционал. Например, вы хотите настроить в своей локальной сети свой собственный сервер времени, чтобы остальные компьютеры сверяли свои часы с ним. В этом случае вам нужна будет служба ntp. А раз вы ее и так поставите, то зачем вам дублирование функционала? В этом случае имеет смысл отключить _timesyncd_ и оставить только _ntp_. Она умеет работать и в качестве сервера времени, и в качестве клиента синхронизации.

    $sudo systemctl enable --now ntp

Проверяем статус синхронизации:

    $timedatectl show

![ntp](screens/ntp.png)





## Part 7. NAT

Для скачивания текстовых редакторов используем команду:

    $sudo apt install <program_name>


Созданные файлы:

![ls](screens/ls.png)

![nano_editor_open](screens/nano_editor_open.png)


Чтобы сохранить сделанные изменения, нажмите _Ctrl+O_. Для выхода из nano нажмите _Ctrl+X_. Если вы выходите из редактора, а файл изменен, nano предложит сохранить файл. Чтобы отказаться от сохранения, просто нажмите _N_, а для подтверждения — _Y_. Редактор запросит имя файла. Просто введите имя, а затем нажмите Enter.

![editor_vim_open](screens/editor_vim_open.png)

Нажмите клавишу _Esc_, это важно, потому что вам необходимо выйти из режима вставки, прежде чем вводить команды выхода. Далее можете вести одну из следующих команд:

* :q - выйти;
* :q! - выйти принудительно;
* :wq - позволяет сохранить и выйти Vim.

![editor_mcedit_open](screens/editor_mcedit_open.png)

В таблицы внизу редактора mcedit указаны основные команды. Сохранение - _F2_, Выход _F10_.



Редактирование без сохранения изменений:

![second_nano](screens/second_nano.png)

Чтобы выйти не сохраняя изменения следует нажать комбинацию клавиш _Ctrl+X_, затем отказаться от сохранения нажав клавишу _N_.

![second_vim](screens/second_vim.png)

Нажмите клавишу _Esc_. Далее "_:q!_" - выйти не сохраняя изменения;

![second_mcedit](screens/second_mcedit.png)

Чтобы выйти из _MCEDIT_ нажмите _F10_ затем стрелочками переключитесь на ячейку "_no_", чтобы отказаться от сохранения.

![Result](screens/result.png)

Как видим содержимое файлов дейсвтительно не изменилось.

---

NANO

Чтобы в редакторе nano выполнить поиск и замену текста используется сочетание клавиш "_Ctrl+\\_".

Нажимте _Ctrl+\\_, введите строку, которую необходимо искать и нажмите клавишу Enter. Затем введите строку, на которую произвести замену и нажмите Enter.

После этого появится предложение по замене первого вхождения вашей строки. Вы можете нажать:
_A_ — Выполнить автоматическую замену всех вхождений строки.
_Y_ — Выполнить замену данной найденной строки (после этого вы переместитесь к следующему в хождению искомой строки).
_N_ — Отменить замену данной строки (после этого вы переместитесь к следующему в хождению искомой строки).
_Ctrl+C_ — Прервать поиск.

До:

![before_nanorror](screens/before_nano.png)

После: 

![after_nano](screens/after_nano.png)


 VIM

Общая форма команды замены следующая:

    :[range]s/{pattern}/{string}/[flags] [count]
 

Команда ищет в каждой строке _[range]_ a _{pattern}_ и заменяет ее на _{string}_. _[count]_ — положительное целое число, умножающее команду.

Если нет _[range]_ и _[count]_, заменяется только шаблон, найденный в текущей строке. Текущая строка — это строка, в которой находится курсор.

Например, чтобы найти первое вхождение строки _‘foo’_ в текущей строке и заменить его на _‘bar’_, вы должны использовать:

    :s/foo/bar/
 

Чтобы заменить все вхождения шаблона поиска в текущей строке, добавьте флаг "_g_":

    :s/foo/bar/g
 

Если вы хотите найти и заменить шаблон во всем файле, используйте символ процента _%_ в качестве диапазона. Этот символ указывает диапазон от первой до последней строки файла:

    :%s/foo/bar/g

До:

![before_vim](screens/before_vim.png)

После:

![after_vim](screens/after_vim.png)

---

 MCEDIT

В _MCEDIT_ для того чтобы заменить текст сначала следует нажать клавишу _F7_ для поиска нужных строк/паттернов, после чего нажамать клавишу _F4_ для указания параметров замены текста

До:

![before_mcedit](screens/before_mcedit.png)

После:

![after_mcedit](screens/after_mcedit.png)



## Part 8. Bonus. Introduction to SSH Tunnels

Обновим репозиторий 

    $sudo apt update

Установим  SSH

    $sudo apt-get install ssh

Установим  OpenSSH

    $sudo apt install openssh-server

Добавим пакет SSH-сервера в автозагрузку:

    $sudo systemctl enable ssh

Открываем порт 2022

    $sudo vim /etc/ssh/sshd_config

![port](screens/port.png)

Перезагрузим сервис

    $sudo systemctl restart sshd


![netstat](screens/netstat.png)

* _netstat_ - служит для отображения состояния сетевого интрфейса.
* _flag t_ - показать _TCP_ соединения.
* _flag a_ - вывод всех активных подключений _TCP_.
* _flag n_ - для отображения _IP_ адресов вместо имен хостов.
* _Proto_ - имя протокола.
* _Recv-Q_ - данные в буфере приема _TCP/IP_.
* _Send-Q_ - данные в буфере отправки _TCP/IP_.
* _Local Adress_ - локальный _IP_ адрес.
* _Foreign Adress_ - внешний _IP_ адрес.
* _State_ - прослушивается порт или нет.


* [В начало](#contents)
