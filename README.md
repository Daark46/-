# Домашнее задание к занятию 2 «Кластеризация и балансировка нагрузки»

### Цель задания
В результате выполнения этого задания вы научитесь:

Настраивать балансировку с помощью HAProxy
Настраивать связку HAProxy + Nginx

### Чеклист готовности к домашнему заданию
Установлена операционная система Ubuntu на виртуальную машину и имеется доступ к терминалу
Просмотрены конфигурационные файлы, рассматриваемые на лекции, которые находятся по ссылке

### Инструкция по выполнению домашнего задания
Сделайте fork репозитория c шаблоном решения к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
Выполните клонирование этого репозитория к себе на ПК с помощью команды git clone.
Выполните домашнее задание и заполните у себя локально этот файл README.md:
впишите вверху название занятия и ваши фамилию и имя;
в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
для корректного добавления скриншотов воспользуйтесь инструкцией «Как вставить скриншот в шаблон с решением»;
при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в инструкции по MarkDown.
После завершения работы над домашним заданием сделайте коммит (git commit -m "comment") и отправьте его на Github (git push origin).
Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

### Задание 1
Запустите два simple python сервера на своей виртуальной машине на разных портах
Установите и настройте HAProxy, воспользуйтесь материалами к лекции по ссылке
Настройте балансировку Round-robin на 4 уровне.
На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

### Ответ 1
HAProxy.cfg
```
global
    daemon
    maxconn 256

defaults
    mode http
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms

frontend http
    bind *:80
    default_backend servers

backend servers
    balance roundrobin
    server server1 localhost:8888 check
    server server2 localhost:9999 check

```

![alt text](https://github.com/Daark46/-/blob/main/1.png)

### Задание 2
Запустите три simple python сервера на своей виртуальной машине на разных портах
Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4
HAproxy должен балансировать только тот http-трафик, который адресован домену example.local
На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.

### Ответ 2
HAProxy.cfg
```
global
    daemon
    maxconn 256

defaults
    mode http
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms

frontend http-in
    bind *:80
    acl host_example_local hdr(host) -i example.local
    use_backend backend_example_local if host_example_local

backend backend_example_local
    balance roundrobin
    mode http
    option http-server-close
    server server1 127.0.0.1:7777 weight 2 check
    server server2 127.0.0.1:8888 weight 3 check
    server server3 127.0.0.1:9999 weight 4 check

```

![alt text](https://github.com/Daark46/-/blob/main/2.png)
