# mkosterin_infra
mkosterin Infra repository


Домашнее задание №3

Самостоятельное задание №1. Для подключения к хосту, не имеющего внешнего IP адреса через промежуточный сервер,
используя SSH Forwarding в одну команду используем возможность интерактивного запуска команд, после присоединения к серверу:
ssh -t -i ~/.ssh/oper -A oper@ext_IP_of_bastion 'ssh int_IP_of_something_server'
Ключ -t важен, без него не будет интерактивного режима.

Самостоятельное задание №2. Подключение к хосту не имеющего внешних связей через bastion одной командой вида 'ssh someinternalhost'.
Для решения данной задачи создадим локальный конфиг ssh клиента пользователя. Путь к файлу:
~/.ssh/config
Внутри создадим alias вида:
Host someinternalhost
  HostName 35.211.213.113
  Port 22
  User oper
  IdentityFile ~/.ssh/oper
  ForwardAgent yes
  RemoteCommand ssh 10.142.0.3
  RequestTTY force

После такой настройки команда 'ssh someinternalhost' будет заводить нас на сервер 10.142.0.3 через bastion без дополнительных
движений и вопросов.


bastion_IP = 35.211.213.113
someinternalhost_IP = 10.142.0.2

