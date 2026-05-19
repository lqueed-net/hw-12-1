# Домашнее задание к занятию «Репликация и масштабирование. Часть 1»

---

### Задание 1

На лекции рассматривались режимы репликации master-slave, master-master, опишите их различия.

Master-Slave (Один ведущий):
Запись - только на Master. Чтение - на Slave или Master. При падении Master запись прекращается. Нужно ручное переключение (или MHA/Orchestrator).
Используется для бекапов или масштабирования чтения

Master-Master (Два активных)
Запись - еа оба узла одновременно, чтение - с любого. Высокая доступность (работоспособность сохраняется при падении одной из реплик).
Главная проблема - конфликты: при одновременной записи одной строки репликация сломается (ошибка дубликата ключа или lost update).

---

### Задание 2

Выполните конфигурацию master-slave репликации, примером можно пользоваться из лекции.

*Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.*
master

<img width="876" height="181" alt="image" src="https://github.com/user-attachments/assets/75692a1b-c6ca-4ff4-a31e-ecc102cec745" />
<img width="542" height="174" alt="image" src="https://github.com/user-attachments/assets/d5e71bb1-f77c-4e7d-a498-4c36973fdc91" />

slave

<img width="719" height="206" alt="image" src="https://github.com/user-attachments/assets/903a2d65-9676-4c3f-868f-86d5df342517" />
<img width="554" height="197" alt="image" src="https://github.com/user-attachments/assets/da77f1ca-9cec-46dd-92de-de6c1e9d4be1" />

Сборка образов
docker build -t mysql_master ./master
docker build -t mysql_slave ./slave

Создание сети
docker network create replication

Запуск контейнеров

docker run -d --name mysql_master --net replication -p 3306:3306 mysql_master

docker run -d --name mysql_slave --net replication -p 3307:3306 mysql_slave

<img width="644" height="411" alt="image" src="https://github.com/user-attachments/assets/0013a6f5-9bca-40fe-91b2-89aa44042b18" />
<img width="672" height="738" alt="image" src="https://github.com/user-attachments/assets/ffb694f4-9076-4d7e-a1bf-6cdcc08c237e" />
