# Инструкция по установке CMS Jommla! 3.10.11 на сервер. Восстановление сайта по архивам CMS Joomla!.
Данная инструкция предназначена для развёртывания и запуска CMS Joomla! на сервере, а также восстановление веб-сайта из архивных файлов CMS Joomla!.
# Установка в Ubuntu Linux 22.04
## 0. Обновление системных пакетов
Обновите системные пакеты на вашем сервере

Запустите терминал сочетанием клавиш __Shift__ + __Alt__ + __T__.
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
В случае если у вас поялвяется ошибка о неизвестном пакете, перейдите по данной [ссылке](https://phoenixnap.com/kb/configure-firewall-with-ufw-on-ubuntu) и следуйте инструкциям.

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
* 
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

## 2. Установка MariaDB
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
Если остались вопросы, перейдите сюда. Найдите точку ввода команды ```sudo mysql_secure_installation``` и проверьте последовательность произведённых шагов.
