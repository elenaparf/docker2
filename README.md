# «Оркестрация группой Docker контейнеров на примере Docker Compose» - Парфенова Елена

## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: ```{"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}```
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
- скачайте образ nginx:1.21.1;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП). 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .

## Решение 1
в папке 

```sh
docker pull nginx:1.21.1
```

```sh
docker build -t custom-nginx:1.0.0 .
```

```sh
docker tag custom-nginx:1.0.0 ysatii/custom-nginx:1.0.0
```

```sh
docker push ysatii/custom-nginx:1.0.0
```

https://hub.docker.com/repository/docker/elenaparf/custom-nginx/general

https://hub.docker.com/repository/docker/elenaparf/custom-nginx/tags/1.0.0/sha256-d0323f74b6af50bb16313c3d751152f61a9d81d2a6faae8cdda9d632db0dab91  

ссылка на [Dockerfile ](https://github.com/ysatii/hw4-docker/tree/main/1)

## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

# Ответ 2
```sh
docker run -d --name "elenaparf-nginx-t2" -p 127.0.0.1:8080:80 custom-nginx:1.0.0
docker rename elenaparf-nginx-t2 custom-nginx-t2
```

![1docker1](https://github.com/user-attachments/assets/c85e2c58-4faa-4abd-b0ee-e858fdbc41c5)
![1docker2](https://github.com/user-attachments/assets/d3564c33-6631-4005-bd07-b03327d05ca5)
![1docker3](https://github.com/user-attachments/assets/dce6b2ba-79d8-4abd-a08a-04f97b29bf6b)
![1docker4](https://github.com/user-attachments/assets/310e5d2b-b692-4dbe-bd88-ea11f1215567)

## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

# ответ 3
![1docker5](https://github.com/user-attachments/assets/94826777-faed-4560-8d90-2bf0d46828ac)
Комбинация Ctrl-C передает контейнеру сигнал SIGINT, тем самым завершает его основной процесс и контейнер останавливается.  

изменим порт на 81 
![1docker7](https://github.com/user-attachments/assets/2bcdaf6b-1772-4d61-bf96-2789d9c15ac9)
![1docker8](https://github.com/user-attachments/assets/b1e5934c-298e-4db3-ae20-e0a2719d69bd)
![1docker9](https://github.com/user-attachments/assets/e554d83a-3684-44f1-a9f6-96b463546be1)

контейнер по прежнему ведет проброс трафика на внутренний 80 порт! но он перестал отвечать. дальнейная работа не возможна нет порта который отвечает на веб запросы

удаляем не останавливая
```sh
docker rm -f custom-nginx-t2
```

## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

# ответ 4
запускаем два контейнера и проверяем? что они в работе

```sh
docker run -d   --name centos-container   -v "$(pwd):/data"   centos tail -f /dev/null
docker run -d   --name debian-container   -v "$(pwd):/data"   debian tail -f /dev/null
docker ps 
```
поключимся к первому контейнеру и создадим файл, проверим что он создался 
```sh
docker exec -it centos-container bash
echo "Hello from CentOS container" > /data/file_in_container.txt
ls /data
cat /data/file_in_container.txt
```
Выйдем  из контейнера командой exit.

Создание файла на хостовой машине
```sh
 echo "Hello from host" > "$(pwd)/file_on_host.txt"
ls "$(pwd)"
cat "$(pwd)/file_on_host.txt"
 ```

Просмотр файлов во втором контейнере
```sh
docker exec -it debian-container bash
ls /data
```
![1docker11](https://github.com/user-attachments/assets/b4e999b3-2b37-4259-b7bb-5dfab813ae3b)


## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2

    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

# ответ 5
1. По умолчанию путь к файлу Compose: compose.yaml (предпочтительный) или compose.yml. Compose также поддерживает docker-compose.yaml и docker-compose.yml для обеспечения обратной совместимости с более ранними версиями. Если в рабочем каталоге оба файла, Compose предпочитает compose.yaml.

![1docker12](https://github.com/user-attachments/assets/88c27d53-c57e-4cbe-a405-5d9d4155e9c4)

2. Отредактируйте файл compose.yaml так, чтобы были запущены оба файла

```
version: "3"

include:
  - docker-compose.yaml

services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
![1docker13](https://github.com/user-attachments/assets/73b5f1e2-7129-44d4-8359-41abe785a5a2)

3. проверим, какие образы имеются на хостовой машине
![1docker14](https://github.com/user-attachments/assets/04f87d04-e635-43a1-83dd-4c9028c120f7)

переименуем образ 
```
docker tag ysatii/custom-nginx:1.0.0 localhost:5000/custom-nginx:latest
```

зальем и проверим его наличие в хранилище

```
docker puch localhost:5000/custom-nginx:latest
```
![1docker15](https://github.com/user-attachments/assets/f96dc305-def3-408b-9416-5fcc6b253bef)

4. Откроем страницу "https://127.0.0.1:9000" и произведем начальную настройку portainer.(логин и пароль адмнистратора)
![1docker16](https://github.com/user-attachments/assets/df5f5d00-76b2-41f0-864d-758a7d7bb881)
![1docker17](https://github.com/user-attachments/assets/b46c81eb-99bf-4b2f-b18e-a3606eaab8a9)
![1docker18](https://github.com/user-attachments/assets/25e9d276-190a-4bce-a8e7-775878a2448b)
![1docker19](https://github.com/user-attachments/assets/32f29e9c-3600-4780-99c7-c6cbca2b3824)

5. Перейдем  на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберем контейнер с nginx и нажмем на кнопку "inspect". В представлении <> Tree развернем поле "Config" и сделаем скриншот от поля "AppArmorProfile" до "Driver".
![1docker20](https://github.com/user-attachments/assets/01cbd447-31ed-43d7-a619-f03f36a33b67)
![1docker21](https://github.com/user-attachments/assets/0f4e1926-d2dc-4224-9cc1-93441ee43927)
![1docker22](https://github.com/user-attachments/assets/07544512-e1ed-48e3-8981-4b401d670396)

6. Удалим любой из манифестов компоуза(например compose.yaml). Выполним команду "docker compose up -d". 
Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.
![1docker22](https://github.com/user-attachments/assets/e58e09fa-e952-4dee-a169-3700654f4400)

Found orphan containers ([task5-portainer-1]) for this project. If you removed or renamed this service in your compose file, you can run this command with the --remove-orphans flag to clean it up
-  проблема в том что есть сироты т.е. контейнеры которые не описаны в манифесте 

-  очищаем контейры командой 
 ```
 docker compose up -d --remove-orphans
 ```
- гасим проект 
```
 docker compose down
```
