# Linux
# Создание пользователя
```
useradd artem -d /home/artem -m -G users -s /bin/bash ; \
passwd artem ; \
sudo usermod -aG sudo artem
```




## Обновление системы и установка необходимых пакетов
```
sudo apt update ; \
sudo apt upgrade -y ; \
sudo apt install -y vim tmux htop git curl wget unzip zip gcc build-essential make net-tools
```
### Установка зависимостей
```
sudo apt install -y tree redis-server nginx zlib1g-dev libbz2-dev libreadline-dev llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev liblzma-dev python3-dev python3-lxml libxslt-dev libffi-dev libssl-dev gnumeric libsqlite3-dev libpq-dev libxml2-dev libxslt1-dev libjpeg-dev libfreetype6-dev libcurl4-openssl-dev supervisor
```




## Оформление
### Установка eza
```
sudo apt install -y gpg
sudo mkdir -p /etc/apt/keyrings
wget -qO- https://raw.githubusercontent.com/eza-community/eza/main/deb.asc | sudo gpg --dearmor -o /etc/apt/keyrings/gierens.gpg
echo "deb [signed-by=/etc/apt/keyrings/gierens.gpg] http://deb.gierens.de stable main" | sudo tee /etc/apt/sources.list.d/gierens.list
sudo chmod 644 /etc/apt/keyrings/gierens.gpg /etc/apt/sources.list.d/gierens.list
sudo apt update
sudo apt install -y eza
```
### Установка Nerd шрифта
```
mkdir -p ~/.local/share/fonts
cd ~/.local/share/fonts && curl -fLO https://github.com/ryanoasis/nerd-fonts/raw/HEAD/patched-fonts/DroidSansMono/DroidSansMNerdFont-Regular.otf
```
### zsh
Установка zsh:
```
sudo apt install -y zsh
```
Установка oh-my-zsh:
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
Смена bash на zsh
```
sudo chsh -s $(which zsh)
```
Установка powerlevel10k:
```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
Добавить в файл `~/.zshrc` тему `powerlevel10k`:
```
ZSH_THEME="powerlevel10k/powerlevel10k"
```
Для запуска настройки темы:
```
p10k configure
```
Добавление `alias ls` в файл `~/.zshrc`:
```
alias ls="eza --tree --level=1 --icons=always --no-time --no-user --no-permissions"
```
Перезапуск zsh:
```
exec zsh
```
### Alacritty
Установка тем
```
# We use Alacritty's default Linux config directory as our storage location here.
mkdir -p ~/.config/alacritty/themes
git clone https://github.com/alacritty/alacritty-theme ~/.config/alacritty/themes
```




## Установка Python 3.10
```
sudo wget https://www.python.org/ftp/python/3.10.11/Python-3.10.11.tgz ; \
tar xvf Python-3.10.* ; \
cd Python-3.10.11 ; \
mkdir ~/.python ; \
./configure --enable-optimizations --prefix=$HOME/.python ; \
make -j8 ; \
sudo make altinstall ; \
rm -rf ~/Python-3.10.11 ; \
rm  -rf ~/Python-3.10.11.tgz
```
Изменение файла `~/.zshrc`:
```
sudo vim ~/.zshrc
```
Добавить следующую строчку:
```
export PATH=$HOME/.python/bin:$PATH
```
Обновление pip и setuptools:
```
pip3.10 install --upgrade pip ; \
pip3.10 install --upgrade setuptools
```




## Мониторинг температуры
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
ssh-copy-id artem@91.223.28.50
```
### Редактирвание файла настройки ssh
Открываем файл настроек:
```
sudo vim /etc/ssh/sshd_config
```
Изменяем следующие параметры:
```
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
X11Forwarding yes
```
Перезапускаем ssh-сервер:
```
sudo service sshd restart
```




## Установка Docker
[Инструкция с официального сайта](https://docs.docker.com/engine/install/ubuntu/)




## Исправление ошибок
### ssh-ключи
Текст ошибки:
```
sign_and_send_pubkey: signing failed for RSA "/home/artem/.ssh/id_rsa" from agent: agent refused operation
artem@192.168.0.103: Permission denied (publickey).
```
Необходимо изменить права доступа к файлам ssh-ключей:
```
sudo chmod 600 ~/.ssh/id_ed25519 ; \
sudo chmod 644 ~/.ssh/id_ed25519.pub
```
### zsh
В случае, если zsh перестала видеть программы (постоянно `command not found`) ввести в терминале следующее:
```
PATH=/bin:/usr/bin:/usr/local/bin:${PATH}
export PATH
```
