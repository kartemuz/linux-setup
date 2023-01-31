# orangepi-server
## Обновление системы
```
sudo apt update ; \
sudo apt full-upgrade -y
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
Перезапускаем ssh сервер:
```
sudo service sshd restart
```
