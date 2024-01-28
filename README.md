# CLI
* Проведите обновление системы
```
su -
apt-get update && apt-get dist-upgrade && update-kernel
или одной командой
epm full-upgrade
```
* Создайте пользователя user2 с паролем P@ssw0rd
```
useradd user2
passwd user2
или через Центр управления системой – меню Пуск – команда acc – Локальные пользователи учетные записи
! Обязательно внимательно посмотри какой пароль, после заверши сеанс, переключись на user2, зайди под новым паролем, если все ок, то заново возвращайся в user
```
* Установите Яндекс браузер, настройте блокировщик рекламы для работы с Яндекс браузером
```
su -
apt-get update 
apt-get install binutils 
apt-get install jq 
apt-get install yandex-browser-stable
зайди в браузер и найди блокировщик, установи
```
* Установите Telegram 
```
apt-get install telegram-desktop
```
* Установите Steam 
```
apt-get install i586-steam
```
* Подключитесь к nextcloud и сохраните учётные данные для входа.
```
??? Ввести в браузере https://10.11.12.10:8080 
```
* Установите антивирус Kaspersky
```
В гугле запрос Касперский скачать линукс и открыть страницу 
О Kaspersky Endpoint Security 11.4.0 для Linux https://support.kaspersky.ru/kes-for-linux/11.4.0
далее Скачать - пролистать вниз до Kaspersky Endpoint Security для Linux найти
Version 12.0.0.6672 | Red Hat Enterprise Linux x64 | Distributive
Version 12.0.0.6672 | Red Hat Enterprise Linux x64 | Product GUI
Прямые ссылки:
https://products.s.kaspersky-labs.com/endpoints/keslinux10/12.0.0.6672/multilanguage-12.0.0.6672/3739343633347c44454c7c31/kesl-12.0.0-6672.x86_64.rpm
https://products.s.kaspersky-labs.com/endpoints/keslinux10/12.0.0.6672/multilanguage-12.0.0.6672/3739343633377c44454c7c31/kesl-gui-12.0.0-6672.x86_64.rpm
затем в гугле Касперский альт линукс открыть страницу Kaspersky Endpoint Security
https://www.altlinux.org/Kaspersky_Endpoint_Security 
в терминал скопировать /opt/kaspersky/kesl/bin/kesl-setup.pl
и много раз Enter и y
```
* Настройте расписание сканирование системы
```
kesl-control --get-task-list и посмотреть что стоит в 1 и 2 пунктах, затем 
kesl-control --get-schedule Scan_My_Computer
/opt/kaspersky/kesl/bin/kesl-control --get-schedule 2 --file /mnt/shed.ini
Зайти в файл /mnt/shed.ini удалить все в нем и скопировать текст
RuleType=Hourly
RunMissedStartRules=No
StartTime=2024/January/25 23:05:00;10
RandomInterval=0
Затем
kesl-control --set-schedule Scan_My_Computer --file /mnt/shed.in
kesl-control --start-t  Scan_My_Computer
kesl-control --start-t  File_Threat_Protection
Проверить 
kesl-control --get-task-list
kesl-control --get-schedule Scan_My_Computer 
```
* Проверить работоспособность антивируса с помощью программы eicar
```
Ввести в гугле тестовая вредоносная программа eicar
И ниже
•	Архив eicar.zip по протоколу HTTPS / HTTP с сервера «Лаборатории Касперского»;
•	Исполняемый файл eicar.com по протоколу HTTPS c официального сайта eicar.org.
попереходить по ссылкам, оставить вкладку
```

* Установите игру «CS 1.6» (установочный файл запрашиваете у экспертов) используя программное обеспечение «Play on Linux» и программное обеспечение wine (их тоже необходимо установить)
```
Сначала установить Wine
apt-get install wine-full i586-wine
https://soft-game.net/load/cs/download_cs/counter_strike_1_6_na_russkom_jazyke/2-1-0-581
только после
apt-get install playonlinux

```
# SRV
Отключение автозапуска графики
```
systemctl set-default multi-user.target
```
Для включения обратно:
```
systemctl set-default graphical.target
```
* Настройте следующий IP адрес 10.11.12.10 с маской 255.255.255.0 (24) шлюз по умолчанию 10.11.12.1 и DNS сервер 10.11.12.1
```
mc
зайти в папку etc/net/ifaces/ens33/
зайти с F4 в файлы:
ipv4address и написать имя ip-адреса и маску:  10.11.12.10/24
ip4route и написать default via и шлюз: default via 10.11.12.1
resolv.conf и написать nameserver и DNS-адрес: nameserver 10.11.12.1
options и сменить dhcp на static: BOOTPROTO=static
systemctl restart network
```
* Проверьте связность между SRV и CLI, а так же доступность в интернет.
```
ping ya.ru
ping и указать ip-адрес CLI (после того, как он установится и команды ip a)  
```
* Проведите обновление системы.
```
apt-get update && apt-get dist-upgrade && update-kernel
перезагрузи вирт машину
```
* Задайте имя компьютера SRV.
```
hostnamectl set-hostname SRV
```
* Поменяйте пароль пользователю user на P@ssw0rd
```
passwd user
! Обязательно перезагрузи SRV и зайди в user заново под новым паролем
```
* Подключите созданную NFS шару с сервера 10.11.12.1. Она должна автоматический монтироваться по пути /mnt/nfsshare
```
Сначала mc
Зайти в папку /mnt/ 
Нажать на F7 (создать новый каталог) и задать имя nfsshare (точное имя и место посмотри в задании)
Затем exit и команду
mount 10.11.12.1:/mysharedir /mnt/nfsshare
Зайди заново в папку и проверь появился ли там файл cstrike-docker.tar
если нет попробуй mount -t nfs 10.11.12.1:/mysharedir /mnt/nfsshare
```
* Настройте подключение по SSH для удалённого конфигурирования устройства
```
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
```
* Необходимо поставить сервер для игры «CS 1.6» выбор пути установки на ваш выбор но мы рекомендуем поставить через docker
```
apt-get install docker-ce
systemctl enable --now docker docker.socket
docker image load -i  /mnt/nfsshare/cstrike-docker.tar
```
* Установите хранилище данных nextcloud
```  Развернуть Nextcloud можно используя пакет deploy (см. Deploy):
Сначала, если еще не выполняли apt-get update
apt-get install deploy
deploy nextcloud
Nextcloud будет установлен в /var/www/webapps/nextcloud, веб-интерфейс будет доступен по ссылке:
http://localhost/nextcloud
Логин - ncadmin 
Пароль можно найти в файле /var/www/webapps/nextcloud/config/config.php в параметре dbpassword.
Нажать справа на кружок, выбрать «Пользователи», слева 
```
* Настройте пользователя USER_RESU с паролем P@ssw0rd для доступа с клиентского компьютера
``` 
Nextcloud будет установлен в /var/www/webapps/nextcloud, веб-интерфейс будет доступен по ссылке:
http://localhost/nextcloud
Логин - ncadmin 
Пароль можно найти в файле /var/www/webapps/nextcloud/config/config.php в параметре dbpassword.
Нажать справа на кружок, выбрать «Пользователи», слева 
```
* Установите Web-интерфейс Алтератор для управления сервером с CLI
```
apt-get install ahttpd alterator-fbi 
systemctl enable --now ahttpd
systemctl enable --now alteratord
затем в браузере на CLI ввести https://10.11.12.10:8080 , далее кнопка "Я принимаю риск" или что-то типа того и ввести логин root пароль toor
```

   
