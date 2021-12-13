# Homework

## Task 1: Users and groups

#### Используйте команды: groupadd, useradd, passwd, chage и другие.
#### Создайте группу sales с GID 4000 и пользователей bob, alice, eve c основной группой sales. 

```bash
[alia2@localhost ~]$ sudo groupadd -g 4000 sales
[alia2@localhost ~]$ useradd -g sales bob
[alia2@localhost ~]$ sudo useradd -g sales alice
[alia2@localhost ~]$ sudo useradd -g sales eve
[alia2@localhost ~]$ cat /etc/passwd
bob:x:1009:4000::/home/bob:/bin/bash
alice:x:1010:4000::/home/alice:/bin/bash
eve:x:1011:4000::/home/eve:/bin/bash
```

#### Измените пользователям пароли.

```bash
[alia2@localhost ~]$ sudo passwd bob
[alia2@localhost ~]$ sudo passwd alice
[alia2@localhost ~]$ sudo passwd eve
```

#### Все новые аккаунты должны обязательно менять свои пароли каждый 30 дней.

#####Для того, чтобы новые аккаунты обязательно меняли свои пароли каждые 30 дней, я изменила значение PASS_MAX_DAYS на 30 в файле /etc/login.defs


#### Новые аккаунты группы sales должны истечь по окончанию 90 дней срока, а bob должен изменять его пароль каждые 15 дней.

```bash
[alia2@localhost ~]$ sudo chage -E `date -d "90 days" +"%Y-%m-%d"` bob
[alia2@localhost ~]$ sudo chage -E `date -d "90 days" +"%Y-%m-%d"` alice
[alia2@localhost ~]$ sudo chage -E `date -d "90 days" +"%Y-%m-%d"` eve
[alia2@localhost ~]$ sudo chage -M 15 bob
```

#### Дополнительно:
#### Заставьте пользователей сменить пароль после первого логина.

```bash
[alia2@localhost ~]$ sudo chage -d 0 bob
[alia2@localhost ~]$ sudo chage -d 0 alice
[alia2@localhost ~]$ sudo chage -d 0 eve
```

## Task 2: Controlling access to files with Linux file system permissions

#### Используйте команды: su, mkdir, chown, chmod и другие.
#### Создайте трёх пользователей glen, antony, lesly

```bash
[alia2@localhost ~]$ sudo groupadd students
[alia2@localhost ~]$ sudo useradd -g students glen
[alia2@localhost ~]$ sudo useradd -g students antony
[alia2@localhost ~]$ sudo useradd -g students lesly
```

#### У вас должна быть директория /home/students, где эти три пользователя могут работать совместно с файлами. Должен быть возможен только пользовательский и групповой доступ, создание и удаление файлов в /home/students. Файлы, созданные в этой директории, должны автоматически присваиваться группе студентов students.


```bash
[alia2@localhost home]$ sudo mkdir /home/students
[alia2@localhost ~]$ sudo chown -R :students /home/students
[alia2@localhost ~]$ sudo chmod -R 2770 /home/students
```

## Task3: ACL

#### Детективное агентство Бейкер Стрит создает коллекцию совместного доступа для хранения файлов дел, в которых члены группы bakerstreet будут иметь права на чтение и запись. Ведущий детектив, Шерлок Холмс, решил, что члены группы scotlandyard также должны иметь возможность читать и писать в общую директорию. Тем не менее, Холмс считает, что инспектор Джонс является достаточно растерянным, и поэтому он должен иметь доступ только для чтения. Миссис Хадсон только начала осваивать Linux и смогла создать общую директорию и скопировать туда несколько файлов. Но сейчас время чаепития, и она попросила вас закончить работу.

#### Используйте команды: touch, mkdir, chgrp, chmod, getfacl, setfacl и другие. 

#### Предварительный шаг:
#### От суперпользователя создайте папку /share/cases и создайте внутри 2 файла murders.txt и moriarty.txt.

```bash
[alia2@localhost /]$ sudo mkdir /share /share/cases
[alia2@localhost ~]$ sudo touch /share/casesmurders.txt /share/casesmoriarty.txt
```

#### Создайте группу bakerstreet с пользователями holmes, watson.

```bash
[alia2@localhost ~]$ sudo groupadd bakerstreet
[alia2@localhost ~]$ sudo useradd -g bakerstreet holmes
[alia2@localhost ~]$ sudo useradd -g bakerstreet watson
[alia2@localhost ~]$ sudo passwd holmes
[alia2@localhost ~]$ sudo passwd watson
```

#### Создайте группу scotlandyard с пользователями lestrade, gregson, jones.

```bash
[alia2@localhost ~]$ sudo useradd -g scotlandyard lestrade
[alia2@localhost ~]$ sudo useradd -g scotlandyard gregson
[alia2@localhost ~]$ sudo useradd -g scotlandyard jones
[alia2@localhost ~]$ sudo passwd lestrade
[alia2@localhost ~]$ sudo passwd gregson
[alia2@localhost ~]$ sudo passwd jones
```

#### Ваша задача - завершить настройку директории общего доступа. Директория и всё её содержимое должно принадлежать группе bakerstreet, при этом файлы должны обновляться для чтения и записи для владельца и группы (bakerstreet). У других пользователей не должно быть никаких разрешений. Вам также необходимо предоставить доступы на чтение и запись для группы scotlandyard, за исключением Jones, который может только читать документы.

```bash
[alia2@localhost ~]$ sudo chown -R holmes:bakerstreet /share/cases
[alia2@localhost ~]$ sudo setfacl -R -m d:g:bakerstreet:rwx /share/cases
[alia2@localhost ~]$ sudo setfacl -m d:g:scotlandyard:rwx /share/cases
[alia2@localhost ~]$ sudo setfacl -m d:u:jones:r-x /share/cases
[alia2@localhost /]$ getfacl /share/cases
# file: /share/cases
# owner: holmes
# group: bakerstreet
# flags: ss-
user::rw-
user:jones:r--
group::rw-
group:scotlandyard:rw-
mask::rw-
other::---
default:user::rw-
default:user:jones:r-x
default:group::rw-
default:group:bakerstreet:rwx
default:group:scotlandyard:rwx
default:mask::rwx
default:other::---
```
