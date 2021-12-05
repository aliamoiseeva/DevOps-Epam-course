# Homework

## ex1

1. Открыть инструкцию по пользованию приложением awk. Найти секцию про использование переменных окружения. Сохранить эту секцию в отдельный файл.

```bash
[alia2@localhost ~]$ man awk
[alia2@localhost ~]$ cat access.log | grep -Po '\"\s\".*\"$' | grep -Po '[^\"]{1,}' | awk -F '"' '{arr[$1]+=1}END{for (ip in arr) print arr[ip], ip}' | uniq |sort -k1n | tail -n3 | head -n1
340874 Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)
```

## ex2

2. Написать скрипт, который создаёт файл "task2.txt" директорией выше своего местоположения. В случае ошибки текст ошибки записывается в err.log а пользователю выдаётся сообщение "Error."

```bash
#!/bin/bash

touch ../task2.txt
if 2>err.log;
        then echo "Error"
fi
```

2*. Если файл уже существует, выдаётся одна ошибка, а если нет прав для его создания - другая.

```bash
#!/bin/bash

a=$(find ../ -name "task2.txt")
touch ../task2.txt 2>err.log
b=$(awk '/Permission denied/ {print "Permission denied"}' err.log)

if [[ $a = '../task2.txt' ]]; then
        echo "The file already exists"
elif [[ $b = 'Permission denied' ]]; then
                echo "Permission denied"
else
        echo "Success"
fi
```

## ex3

3. Создать 2 файла: 1-й - текстовый с указанием абслютного пути до директории. 2-й - скрипт, который при выполнении выводит содержимое директории по указанному в первом файле.

```bash
[alia2@localhost new]$ pwd > test2.txt
#!/bin/bash

a=$(cat test2.txt)
ls -R $a
```

3*. Скрипт выводит отдельно количество файлов и количество директорий.

```bash
#!/bin/bash

a="/home/alia2/new"
ls -R $a > $a/test.txt
files=$(find "$a" -type f | wc -l )
folders=$(find "$a" -type d | wc -l )

echo "Files: $files" >> $a/test.txt
echo "Folders: $folders" >> $a/test.txt
```

3**. Скрипт принимает любое количество записей в первом файле и обрабатывает их последовательно.
(Сделала как поняла, если требуется, чтобы скрипт выполнял другую задачу, распишите, пожалуйста, что конкретно. Из задания непонятно.)

```bash
#!/bin/bash

a="/home/alia2/new"
count=1
while [ -n "$1" ]
do
echo "Parametor #$count = $1" >> $a/test.txt
count=$[ $count + 1 ]
shift
done

[alia2@localhost new]$ ./task3*.script1.sh 76 87 67
[alia2@localhost new]$ cat test.txt
Parametor #1 = 76
Parametor #2 = 87
Parametor #3 = 67
```

