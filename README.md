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
### zsh
Установка zsh:
```
sudo apt install zsh
```
Установка oh-my-zsh:
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
Смена bash на zsh
```
chsh -s $(which zsh)
```
### Установка Python 3.10
```
sudo wget https://www.python.org/ftp/python/3.10.13/Python-3.10.13.tgz ; \
tar xvf Python-3.10.* ; \
cd Python-3.10.13 ; \
mkdir ~/.python ; \
./configure --enable-optimizations --prefix=/home/artem/.python ; \
make -j8 ; \
sudo make altinstall
```
Изменение файла `~/.zshrc`:
```
sudo vim ~/.zshrc
```
Добавить следующую строчку:
```
export PATH=/home/artem/.python/bin:$PATH
```
Обновление pip и setuptools:
```
pip3.10 install --upgrade pip ; \
pip3.10 install --upgrade setuptools
```



### Мониторинг температуры
Установка:
```
sudo apt install lm-sensors
```
Запуск мастера настройки lm_sensors:
```
sudo sensors-detect
```
Отвечайте Y на все вопросы. Утилита попытается обнаружить все доступные в системе встроенные аппаратные датчики (для процессора, видеокарты, памяти и других микросхем), а также автоматически определить подходящие драйвера для них.
Добавляем lm_sensors в автозагрузку:
```
sudo systemctl enable lm-sensors
```
Проверка температуры процессора и других аппаратных компонентов:
```
sensors
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
ssh-copy-id artem@192.168.0.103
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
## Исправление ошибок
### ssh-ключи
Текст ошибки:
```
sign_and_send_pubkey: signing failed for RSA "/home/artem/.ssh/id_rsa" from agent: agent refused operation
artem@192.168.0.103: Permission denied (publickey).
```
Необходимо изменить права доступа к файлам ssh-ключей:
```
sudo chmod 600 ~/.ssh/id_rsa ; \
sudo chmod 644 ~/.ssh/id_rsa.pub
```
### zsh
В случае, если zsh перестала видеть программы (постоянно `command not found`) ввести в терминале следующее:
```
PATH=/bin:/usr/bin:/usr/local/bin:${PATH}
export PATH
```
