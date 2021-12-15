# Homework

## ex1

1. Открыть инструкцию по пользованию приложением awk. Найти секцию про использование переменных окружения. Сохранить эту секцию в отдельный файл.

```bash
[alia2@localhost ~]$ man awk
[alia2@localhost ~]$ man awk | grep -C10 'ENVIRONMENT VARIABLES' | tail -11 > test.txt
[alia2@localhost ~]$ cat test.txt
ENVIRONMENT VARIABLES
       The  AWKPATH environment variable can be used to provide a list of directories that gawk searches when looking
       for files named via the -f and --file options.

       For socket communication, two special environment variables can be used  to  control  the  number  of  retries
       (GAWK_SOCK_RETRIES),  and the interval between retries (GAWK_MSEC_SLEEP).  The interval is in milliseconds. On
       systems that do not support usleep(3), the value is rounded up to an integral number of seconds.

       If POSIXLY_CORRECT exists in the environment, then gawk behaves exactly as if --posix had  been  specified  on
       the command line.  If --lint has been specified, gawk issues a warning message to this effect.

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
        echo "Done"
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

a=$(cat test2.txt)
ls -R $a
files=$(find "$a" -type f | wc -l )
folders=$(find "$a" -type d | wc -l )

echo "Files: $files"
echo "Folders: $folders"
```

3**. Скрипт принимает любое количество записей в первом файле и обрабатывает их последовательно.

```bash
#!/bin/bash

a=$(cat test2.txt)
for var in $(ls -R $a)
do
echo "$var"
done
```

