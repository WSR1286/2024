# 2024

**1.	Установка операционной системы на CLI: **
a.	Установите операционную систему Альт Рабочая станция,
b.	Задайте имя компьютера CLI,
1) Откройте терминал
# su – 
2) Введите пароль администратора, например toor  
# hostnamectl set-hostname CLI
3) Перезагрузите компьютер
c.	При установке укажите имена пользователь user с паролем resu, и пользователь root с паролем toor,
d.	IP адрес вам приход от провайдера по DHCP,
e.	Остальные настройки при установки оставить по умолчанию.

Пока устанавливается операционная система на CLI перейдём к настройке SRV
Отключение автозапуска графики
# systemctl set-default multi-user.target
Для включения обратно:
# systemctl set-default graphical.target
2.	На SRV необходимо сделать следующие настройки: 
Текущие учетные данные пользователь user с паролем resu, и пользователь root с паролем toor.
Узнать свой ip-адрес: команда ip a
a.	Настройте следующий IP адрес 10.11.12.10 с маской 255.255.255.0 (24) шлюз по умолчанию 10.11.12.1 и DNS сервер 10.11.12.1
b.	Проверьте связность между SRV и CLI, а так же доступность в интернет.
mc
зайти в папку etc/net/ifaces/ens33/
зайти с F4 в файлы: 
-	ipv4address и написать имя ip-адреса и маску:  10.11.12.10/24
-	ip4route и написать default via и шлюз: default via 10.11.12.1
-	resolv.conf и написать nameserver и DNS-адрес: nameserver 10.11.12.1
-	options и сменить dhcp на static: BOOTPROTO=static
ввести команду systemctl restart network
https://www.altlinux.org/Настройка_сети 
ping ya.ru
ping и ввести ip-адрес CLI (который мы узнаем через ip a в терминале клиента)
c.	Проведите обновление системы.
epm full-upgrade
d.	Задайте имя компьютера SRV. 
hostnamectl set-hostname SRV
e.	Поменяйте пароль пользователю user на P@ssw0rd.
passwd user
f.	Подключите созданную NFS шару с сервера 10.11.12.1. Она должна автоматический монтироваться по пути /mnt/nfsshare.
Сначала mc
Зайти в папку /mnt/ 
Нажать на F7 (создать новый каталог) и задать имя nfsshare
Затем exit и команду
mount 10.11.12.1:/mysharedir /mnt/nfsshare
g.	Настройте подключение по SSH для удалённого конфигурирования устройства.
Сначала, если еще не выполняли apt-get update
apt-get install openssh-server
systemctl enable sshd.service
systemctl status sshd 
mc
перейти в папку …/etc/openssh/sshd_config
F4
Раскоментировать строки Port22 и permitRootlogin 
В последней убрать without_password и написать yes
F2
Нажать на F10 мышкой в прав нижнем углу
Ввести exit 
systemctl restart sshd

h.	Необходимо поставить сервер для игры «CS 1.6» выбор пути установки на ваш выбор но мы рекомендуем поставить через docker.
apt-get install docker-ce
systemctl enable --now docker docker.socket
docker image load -i  /mnt/nfsshare/cstrike-docker.tar

i.	Установите хранилище данных nextcloud.
Развернуть Nextcloud можно используя пакет deploy (см. Deploy):
Сначала, если еще не выполняли apt-get update
apt-get install deploy
deploy nextcloud
Nextcloud будет установлен в /var/www/webapps/nextcloud, веб-интерфейс будет доступен по ссылке:
http://localhost/nextcloud
Логин - ncadmin 
Пароль можно найти в файле /var/www/webapps/nextcloud/config/config.php в параметре dbpassword.
Нажать справа на кружок, выбрать «Ползователи», слева 
i.	Настройте пользователя USER_RESU с паролем P@ssw0rd для доступа с клиентского компьютера
j.	Установите Web-интерфейс Алтератор для управления сервером с CLI.
Альтератор – это Центр управления системой
Сначала, если еще не выполняли apt-get update
apt-get install ahttpd alterator-fbi 
systemctl enable --now ahttpd
systemctl enable --now alteratord
затем в браузере на CLI ввести 127.0.0.1:8080 (вместо ip ввести ip-адрес сервера)

3.	Настройка клиентского компьютера:
a.	Проведите обновление системы.
apt-get update && apt-get dist-upgrade && update-kernel
или одной командой
epm full-upgrade
b.	Создайте пользователя user2 с паролем P@ssw0rd.
useradd user2 – создание
passwd user2 -пароль
или через Центр управления системой – меню Пуск – команда acc – Локальные пользователи учетные записи
c.	Установите Яндекс браузер.
$ su -
2.1. Перед установкой, необходимо обновить индекс пакетов в режиме супер-пользователя: # apt-get update 
2.2. Для корректной установки Яндекс Браузер необходимо установить зависимости binutils и jq 
# apt-get install binutils 
# apt-get install jq 
# apt-get install yandex-browser-stable
d.	Настройте блокировщик рекламы для работы с Яндекс браузером

i.	Подключитесь к nextcloud и сохраните учётные данные для входа.
Ввести в браузере https://10.11.12.10:8080 
e.	Установите антивирус Kaspersky.
В гугле запрос Касперский скачать линукс и открыть страницу 
О Kaspersky Endpoint Security 11.4.0 для Linux https://support.kaspersky.ru/kes-for-linux/11.4.0 далее Скачать - пролистать вниз до Kaspersky Endpoint Security для Linux найти
Version 12.0.0.6672 | Red Hat Enterprise Linux x64 | Distributive
Version 12.0.0.6672 | Red Hat Enterprise Linux x64 | Product GUI
Прямые ссылки:
https://products.s.kaspersky-labs.com/endpoints/keslinux10/12.0.0.6672/multilanguage-12.0.0.6672/3739343633347c44454c7c31/kesl-12.0.0-6672.x86_64.rpm
https://products.s.kaspersky-labs.com/endpoints/keslinux10/12.0.0.6672/multilanguage-12.0.0.6672/3739343633377c44454c7c31/kesl-gui-12.0.0-6672.x86_64.rpm
затем в гугле Касперский альт линукс открыть страницу Kaspersky Endpoint Security https://www.altlinux.org/Kaspersky_Endpoint_Security 
в терминал скопировать /opt/kaspersky/kesl/bin/kesl-setup.pl
и много раз Enter и y
i.	Настройте расписание сканирование системы.
Написать команду 
kesl-control --get-task-list и посмотреть что стоит в 1 и 2 пунктах, затем 
kesl-control --get-schedule Scan_My_Computer
/opt/kaspersky/kesl/bin/kesl-control --get-schedule 2 --file /mnt/shed.ini
Зайти в файл /mnt/shed.ini удалить все в нем и скопировать текст
RuleType=Hourly
RunMissedStartRules=No
StartTime=2024/January/25 23:05:00;10
RandomInterval=0
kesl-control --start-t  Scan_My_Computer
kesl-control --start-t  File_Threat_Protection
Проверить 
kesl-control --set-schedule 2 --file /mnt/shed.in
kesl-control --get-task-list
kesl-control --get-schedule Scan_My_Computer 
Чтобы запустить задачу, выполните следующую команду: /opt/kaspersky/kesl/bin/kesl-control --start-task |
https://support.kaspersky.com/KES4Linux/11.1.0/ru-RU/199938.htm и Kaspersky Endpoint Security for Linux 
ii.	Проверить работоспособность антивируса с помощью программы eicar.
Ввести в гугле тестовая вредоносная программа eicar
И ниже
•	Архив eicar.zip по протоколу HTTPS / HTTP с сервера «Лаборатории Касперского»;
•	Исполняемый файл eicar.com по протоколу HTTPS c официального сайта eicar.org.
попереходить по ссылкам
f.	Установите Telegram (они должны запускается, но не авторизовывайтесь)
apt-get install telegram-desktop
g.	Установите Steam (они должны запускается, но не авторизовывайтесь)
apt-get install i586-steam
h.	Установите игру «CS 1.6» (установочный файл запрашиваете у экспертов) используя программное обеспечение «Play on Linux» и программное обеспечение wine (их тоже необходимо установить)
i.	После установки запустите игру и подключитесь к своему серверу 
apt-get install playonlinux
apt-get install wine-full i586-wine
https://soft-game.net/load/cs/download_cs/counter_strike_1_6_na_russkom_jazyke/2-1-0-581
