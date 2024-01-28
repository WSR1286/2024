# CLI
```
su
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
