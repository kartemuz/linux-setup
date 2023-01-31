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
Открываем файл локализации: `sudo nano /etc/default/locale`
Удаляем всё и вставляем:
```
LC_MESSAGES=ru_RU.UTF-8
LANGUAGE=ru_US.RUF-8
LANG=ru_RU.UTF-8
LC_ALL=ru_RU.UTF-8
```
Выходим нажатием `Ctrl-x`, подтверждая сохранение.
