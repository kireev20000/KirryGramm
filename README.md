# Проект KirryGramm
Мы построили свой инстаграм с блекджеком и котиками!  
***
# Технологии
- Python 3.9
- Django 3.2
- SQLite
- Django Rest Framework 3.12.4
- ReactJS
***
## Запуск проекта:
Клонировать репозиторий
```sh
git clone <ssh ссылка>
```
Cоздать и активировать виртуальное окружение:
```
python -m venv venv
source venv/Scripts/activate
```
Установить зависимости из файла requirements.txt:
```
pip install -r requirements.txt
```
Выполнить миграции 
```
python manage.py migrate
```
Создать суперпользователя
```
python manage.py createsuperuser
```
Заходим в Settings.py и меняем:
```
ALLOWED_HOSTS = [<ваше значение>, '127.0.0.1', 'localhost',]
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / 'static'
MEDIA_URL = '/media/'
MEDIA_ROOT = '/var/www/kittygram/media/' #ваш путь
```
***
### Настройка Gunicorn
Идем в каталог /etc/systemd/system/ и создаем файл файл
```
nano /etc/systemd/system/gunicorn_kittygram.service
```
Копируем туда настройки и изменяем необходимые параметры:
```
[Unit]
Description=gunicorn-kittygram daemon
After=network.target

[Service]
User=yc-user
WorkingDirectory=/home/yc-user/infra_sprint1/backend/ # тут ваш путь

ExecStart=/home/yc-user/infra_sprint1/backend/venv/bin/gunicorn --bind 0.0.0.0:8080 kittygram_backend.wsgi # тут ваш путь

[Install]
WantedBy=multi-user.target
```
Запускаем и автоматизируем запуск демона.
```
sudo systemctl start gunicorn_kittygram
sudo systemctl enable gunicorn_kittygram 
sudo systemctl status gunicorn
```
### Настройка Nginx

Обновите список репозиториев.
```
apt-get update
```
Установите пакет nginx.
```
apt-get install nginx
```
Редактируем файл настроек Nginx (пример наcтроек можно посмотреть в infra/default)
```
sudo nano /etc/nginx/sites-enabled/default
```
Запускаем Nginx
```
sudo service nginx start
```
### Получаем сертификат SSL для домена
Установка пакетного менеджера snap.
```
sudo apt install snapd
sudo snap install core; sudo snap refresh core
```
Установка пакета certbot.
```
sudo snap install --classic certbot
```
Создание ссылки на certbot в системной директории, чтобы у пользователя с правами администратора был доступ к этому пакету.
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot 
```
Чтобы начать процесс получения сертификата, введите команду:
```
sudo certbot --nginx
```
Перезагрузите конфигурацию Nginx.
```
sudo systemctl reload nginx 
```


