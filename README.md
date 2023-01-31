# orangepi-server
## Обновление системы и установка необходимых пакетов
```
sudo apt update ; \
sudo apt full-upgrade -y ; \
sudo apt install -y vim tmux htop git curl wget unzip zip gcc build-essential make
```
### Установка зависимостей
```
sudo apt install -y tree redis-server nginx zlib1g-dev libbz2-dev libreadline-dev llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev liblzma-dev python3-dev python3-lxml libxslt-dev libffi-dev libssl-dev gnumeric libsqlite3-dev libpq-dev libxml2-dev libxslt1-dev libjpeg-dev libfreetype6-dev libcurl4-openssl-dev supervisor
```




## Локализация сервера
```
sudo apt install language-pack-ru language-pack-ru-base
```
### Изменение файла локализации
Открываем файл локализации:
```
sudo vim /etc/default/locale
```
Удаляем всё и вставляем:
```
LC_MESSAGES=ru_RU.UTF-8
LANGUAGE=ru_US.RUF-8
LANG=ru_RU.UTF-8
LC_ALL=ru_RU.UTF-8
```
Перезапускаем сервер:
```
sudo reboot
```




## Настройка ssh
### Размещение открытого ключа на сервере
На ПК клиента запускаем следующую команду, чтобы перенести публичный ключ на удалённый сервер:
```
ssh-copy-id artem@192.168.31.121
```
### Редактирвание файла настройки ssh
Открываем файл настроек:
```
sudo vim /etc/ssh/sshd_config
```
Изменяем следующие параметры:
```
#Port 22
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
X11Forwarding yes
```
Перезапускаем ssh-сервер:
```
sudo service sshd restart
```
## Исправление ошибки "Permission denied (publickey)"
В случае следующей ошибки:
```
sign_and_send_pubkey: signing failed for RSA "/home/artem/.ssh/id_rsa" from agent: agent refused operation
artem@192.168.31.121: Permission denied (publickey).
```
необходимо изменить права доступа к файлам ssh-ключей:
```
sudo chmod 600 ~/.ssh/id_rsa ; \
sudo chmod 644 ~/.ssh/id_rsa.pub
```
