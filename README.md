# mkosterin_infra
mkosterin Infra repository

h2 Домашнее задание №7
h3 Обязательная часть
* изучена работа с firewall через terraform
* изучена возможность импорта частей конфигурации из инфраструктуры в tfstate
* изучены не явные и явные взаимосвязи(зависимости) ресурсов
* созданы модули описывающие инфраструктуру базы данных, приложения, vpc
* созданы две инфраструктуры prod и stage
* к инфраструктуре подключен модуль из реестра для создания bucket
h3 Самостоятельное задание
* написан модуль описывающий настройки firewall
* модуль vpc переделан с учетом передачи параметра
h3 Задание СО*
* создан bucket для хранения tfstate
* описаны два backend для stage и prod с разными префиксами
* проверена работа terraform без локального файла tfstate, проверено создание lock файла и работа блокировок
h3 Задание СО*
* написаны provision модули для деплоя приложения. Файлы окружения находятся в рабочем каталоге модуля
* реализовано определение внутреннего адреса сервера базы данных, его передача на сервер приложения
* в зависиомости от содержимого переменной deploy_app разворачивается или нет приложение ruby
 
h2 Домашнее задание №6

h3 Обязательная часть:
* средствами terraform создан инстанс из шаблона reddit-base
* изучены способы достпа к содержимому tfstate
* настроена output var для определения внешнего IP адреса
* настроен ресурс firewall для открытия порта приложения
* изучен параметры connection для работы provisioners
* изучены provisiners, копирование файлов, выполнение файлов
* изучены базовы команды terraform в командной строке
* созданы входные переменные, подставлены в описание инфраструктуры, определен файл переменных
h3 Самостоятельные задания:
* определил input переменную для приватного ключа
* определил input переменную для задания зоны в ресурсе со значением по умолчанию
* отформатировал конфигурационные файлы terraform
* написал файл terraform.tfvars.example
h3 Задание со 1
* добавил public key пользователя appuser1 в **метаданных проекта**
* описал в коде terraform создание public key трех пользователей в **метаданных проекта**
* исследовал вопрос судьбы ключей, которые были созданы через web интерфейс gcp. Они исчезают при выполнении terraform apply, остаются только те, которые были описаны в коде terraform.
h3 Задание со 2
* Написана инфраструктура балансировщика. Недостатки - отсутствие единой базы данных приложения, соответственно конифгурация годится только как тестовое задание


****
h2 Домашнее задание №5

h3 Обязательная часть:
* Создан шаблон, содержащие базовые компоненты для приложения;
* json шаблон packera сделан параметрическим;
* Изучена тема provisioner типа shell, file;
* Повторно использованы скрипты деплоя приложений;
* Создана тестовая машина из шаблона, на ней вручную развернуто приложение;
* Самостотельные задания:
* Добавлены параметры в шаблон;
* Создан файл переменных;
* Изучены дополнительные параметры в шаблонах
* На основе базового шаблона создан полный шаблон, в котором задеплоено приложение;
* Написан systemd unit файл, организована его доставка в шаблон;
* Написан скрипт, разворачивающий виртульную машину из полного шаблона.


****
h2 Домашнее задание №4
Команда для автоматического деплоя приложения:
```
gcloud compute instances create reddit-app \
--boot-disk-size=10GB \
--image-family ubuntu-1604-lts \
--image-project=ubuntu-os-cloud \
--machine-type=g1-small \
--tags puma-server \
--restart-on-failure \
--metadata-from-file startup-script=startup.sh
```
Команда для создания правила в Брэндмауэре:
```
gcloud compute firewall-rules create default-puma-server \
--allow tcp:9292 \
--source-ranges 0.0.0.0/0 \
--target-tags puma-server

testapp_IP = 35.231.128.85
testapp_port = 9292
```

****
h2 Домашнее задание №3

Самостоятельное задание №1. Для подключения к хосту, не имеющего внешнего IP адреса через промежуточный сервер,
используя SSH Forwarding в одну команду используем возможность интерактивного запуска команд, после присоединения к серверу:
```
ssh -t -i ~/.ssh/oper -A oper@ext_IP_of_bastion 'ssh int_IP_of_something_server'
```
Ключ -t важен, без него не будет интерактивного режима.
***
Самостоятельное задание №2. Подключение к хосту не имеющего внешних связей через bastion одной командой вида 'ssh someinternalhost'.
Для решения данной задачи создадим локальный конфиг ssh клиента пользователя. Путь к файлу:
~/.ssh/config
Внутри создадим alias вида:
````
Host someinternalhost
  HostName 35.211.213.113
  Port 22
  User oper
  IdentityFile ~/.ssh/oper
  ForwardAgent yes
  RemoteCommand ssh 10.142.0.3
  RequestTTY force
```
***
После такой настройки команда 'ssh someinternalhost' будет заводить нас на сервер 10.142.0.3 через bastion без дополнительных
движений и вопросов.


bastion_IP = 35.211.213.113
someinternalhost_IP = 10.142.0.2

