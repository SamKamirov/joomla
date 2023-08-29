# Инструкция по установке CMS Jommla! Восстановление сайта по архивам CMS Joomla!.
Данная инструкция предназначена для развёртывания и запуска CMS Joomla! 3.10.11 на сервере в ОС Linux Ubuntu 22.04, а также восстановление веб-сайта из архивных файлов CMS Joomla!.
# Установка в Ubuntu Linux 22.04
## 0. Обновление системных пакетов
Обновите системные пакеты на вашем сервере

Запустите терминал сочетанием клавиш __Shift__ + __Alt__ + __T__ или любым другим известным вам способом.
```
sudo apt update
sudo apt upgrade
```
## 1. Установка Apache
```
sudo apt install apache2
```
Вам будет предложено подтвердить установку Apache. Подтвердите введя __Y__, затем нажмите клавишу __Enter__.

После завершения установки, вам нужно изменить настройки файервола (брандмауэра).
```
sudo ufw app list
```
В случае если у вас появляется ошибка о неизвестном пакете, перейдите по данной [ссылке](https://phoenixnap.com/kb/configure-firewall-with-ufw-on-ubuntu) и следуйте инструкциям.

Далее снова введите команду.
```
sudo ufw app list
```
Вы увидите примерно следующее.
```
Output
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
```
Значения данных профилей:
* Apache: данный профиль открывает только порт 80 (обычный, зашифрованый веб трафик).
* Apache Full: данный профиль открывает порты 80 и 443 (TLS/SSL зашифрованый трафик).
* Apache Secure: этот профиль открывает только порт 443.

Разрешите подключения по порту 80, так как это свежая установка Apache и у вас еще нет TLS/SSL сертификата для HTTPS трафика на вашем сервере.

Для того, чтобы разрешить только на порту 80, используйте __Apache__ профиль.
```
sudo ufw allow in "Apache"
```
Подтвердите изменения:
```
sudo ufw status
```
Теперь трафик через порт 80 разрешён. Вы можете проверить это введя в IP адресс своего сервера в поисковую строку браузера.

## 2. Установка и настройка MariaDB 10.6
Для установки введите команду:
```
apt install mariadb-server-10.6
```
После установки проверьте работу сервиса:
```
systemctl status mysql
```
Запустите mysql:
```
sudo mysql
```
Введите команду:
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
__root__ и __password__ вы можете заменить на любые значения.
Запустите скрипт:
```
sudo mysql_secure_installation
```
Внимательно прочитайте описания и выберите необходимые вам варианты.
Если остались вопросы, перейдите [сюда](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-22-04). Найдите точку ввода команды ```sudo mysql_secure_installation``` и проверьте последовательность произведённых шагов.

Запустите ```mysql```
```
sudo mysql
```
Создайте пользователя
```
mysql> create user 'your_user'@'localhost' identified with by 'your_password';
```
Создайте базу данных которую будете использовать в дальнейшем.
```
mysql> create database name_of_your_database;
```
Передайте права для созданной вами базы данных созданному пользователю
```
mysql> grant all on example_database.* to 'example_user'@'%';
```
Данная команда выдаст все разрешения и права для управления базой данных, которую вы указали.
Выйдите из ```mysql```
```
mysql> exit
```
## 3. Установка PHP 5.6
Установите пакет ```software-properties-common```
```
sudo apt update
sudo apt install software-properties-common
```
Добавьте репозиторий на сервер
```
sudo add-apt-repository ppa:ondrej/php
```
Обновите индекс установленного репозитория
```
sudo apt update
sudo apt upgrade
```
Установите PHP 5.6
```
sudo apt install -y php5.6
```
Установите дополнительные пакеты 
```
sudo apt-get install php5.6-gd php5.6-mysql php5.6-imap php5.6-curl php5.6-intl php5.6-pspell php5.6-recode php5.6-sqlite3 php5.6-tidy php5.6-xmlrpc php5.6-xsl php5.6-zip php5.6-mbstring php5.6-soap php5.6-opcache php5.6-common php5.6-json php5.6-readline php5.6-xml
```
Перейдите в директорию ```/etc/apache2/mods-enabled/dir.conf```
```
sudo nano /etc/apache2/mods-enabled/dir.conf
```
Впишите ```index.php``` перед ```index.html```
```
<IfModule mod_dir.c>
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
Перезагрузите Apache
```
sudo systemctl reload apache2
```
## 4.Установка Joomla! 3.10
Распакуйте архив Joomla! в директорию ```/var/www/html/``` через FTP клиент. FileZilla - хороший выбор.
Выдайте папке ```/var/www/html/``` все права
```
chmod -R 777 /var/www/html/
```
Перейдите в директорию ```/etc/mysql/mariadb.conf.d```
```
cd /etc/mysql/mariadb.conf.d
```
Откройте файл ```50-server.cnf``` и раскомментируйте строку ```max_allowed_packet```
```
sudo nano 50-server.cnf
```
Перейдите в браузер и введите ```IP адрес``` вашего сервера в строку поиска. Если все шаги выполнены верно, вы должны увидеть окно графиеского установщика.

Установите Joomla! используя графический установщик.

## 5.Восстановление сайта по архивным файлам
Скачайте [Akeeba Kickstart 7.2.0](https://www.akeeba.com/download/akeeba-kickstart/7-2-0/kickstart-core-7-2-0-zip.zip) и распакуйте его в папку где установлена Joomla!

Запустите скрипт введя в адресной строке ```IP адрес сервера/kickstart.php```

Выполните шаги по восстановлению сайта из файлов.

