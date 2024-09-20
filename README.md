## Задача №1
## Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd (вам понадобится grep).
```grep '.*' /etc/passwd | cut -d: -f1 | sort```
```
adm
at
bin
cron
cyrus
daemon
dhcp
```
## Задача №2
## Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов, как показано в примере ниже:
```
[root@localhost etc]# cat /etc/protocols ...
142 rohc
141 wesp
140 shim6
139 hip
138 manet
```
## Решение
``` awk '{print $3, $1}' /etc/protocols | sort -nr | head -n 5```
```
protocols,v #
for #
XTP xtp
XNS-IDP xns-idp
VMTP vmtp
```
## Задача №3
## Написать программу banner средствами bash для вывода текстов, как в следующем примере (размер баннера должен меняться!):
```
[root@localhost ~]# ./banner "Hello from RTU MIREA!"
+-----------------------+
| Hello from RTU MIREA! |
+-----------------------+
```
## Решение 
```
#!/bin/bash

if [ -z "$1" ]; then
  echo "Ошибка: требуется ввести текст для баннера!"
  exit 1
fi

text="$1"

text_length=${#text}

total_length=$((text_length + 4))

border=$(printf "+%${total_length}s+" | tr ' ' '-')

echo "$border"
printf "| %s |\n" "$text"
echo "$border"
+----------------------+
| Hello from RTU MIREA |
+----------------------+
```
## Задача №4
## Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).
## Решение
```grep -oE '\b[a-zA-Z_][a-zA-Z0-9_]*\b' hello.c | sort | uniq```
```
h
hello
include
int
main
n
printf
return
stdio
void
world
```
## Задача №5
## Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin).
## Решение
```
#!/bin/bash
if [ -z "$1" ]; then ли аргумент
        echo "Ошибка: требуется указать имя команды!"
        exit 1
fi
 
command=$1
 
if [ ! -f "$command" ]; then
        echo "Ошибка: файл $command не существует!"
        exit 1
fi
sudo cp "$command" /usr/local/bin/
sudo chmod +x /usr/local/bin/"$command"
```
```'banner' has been registered successfully.```
## Задача №6
## Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.
#!/bin/bash

# Проверка наличия аргументов
if [ "$#" -eq 0 ]; then
    echo "Usage: $0 <file1> <file2> ... <fileN>"
    exit 1
fi

# Проверка каждого переданного файла
for file in "$@"; do
    # Проверка расширения файла
    if [[ "$file" =~ \.(c|js|py)$ ]]; then
        # Проверка существования файла
        if [ ! -f "$file" ]; then
            echo "$file: File not found!"
            continue
        fi

        # Чтение первой строки файла
        first_line=$(head -n 1 "$file")
        
        # Проверка комментария в первой строке
        if [[ "$file" == *.c ]]; then
            if [[ "$first_line" =~ ^\s*// ]]; then
                echo "$file: Comment found"
            else
                echo "$file: No comment in the first line"
            fi
        elif [[ "$file" == *.js ]]; then
            if [[ "$first_line" =~ ^\s*// ]]; then
                echo "$file: Comment found"
            else
                echo "$file: No comment in the first line"
            fi
        elif [[ "$file" == *.py ]]; then
            if [[ "$first_line" =~ ^\s*# ]]; then
                echo "$file: Comment found"
            else
                echo "$file: No comment in the first line"
            fi
        fi
    else
        echo "$file: Unsupported file type"
    fi
done
```
No comment in the first line
Comment found
Comment found
```
## Задача №7
## Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).
## Решение
```
#!/bin/bash

# Проверка наличия аргумента
if [ "$#" -ne 1 ]; then
    echo "Usage: $0 <directory>"
    exit 1
fi

# Путь к директории
directory=$1

# Проверка существования директории
if [ ! -d "$directory" ]; then
    echo "Directory not found!"
    exit 1
fi

# Находим все файлы и их хэши
find "$directory" -type f -exec md5sum {} + | \
    sort | \
    uniq -d -w32 --all-repeated=separate
```
```
d41d8cd98f00b204e9800998ecf8427e
d41d8cd98f00b204e9800998ecf8427e
d41d8cd98f00b204e9800998ecf8427e
```
## Задание №8
## Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.
## Решение 
```
#!/bin/bash

# Проверка наличия двух аргументов
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <directory> <extension>"
    exit 1
fi

# Путь к директории и расширение файлов
directory=$1
extension=$2

# Проверка существования директории
if [ ! -d "$directory" ]; then
    echo "Directory not found!"
    exit 1
fi

# Формирование имени архива
archive_name="archive_$(date +%Y%m%d%H%M%S).tar"

# Поиск файлов с указанным расширением и создание архива
find "$directory" -type f -name "*.$extension" | tar -cvf "$archive_name" -T -

# Проверка успешности создания архива
if [ $? -eq 0 ]; then
    echo "Archive created successfully: $archive_name"
else
    echo "Failed to create archive."
    exit 1
fi
```
```
Archive created successfully: archive_20230919123045.tar
Archive created successfully: archive_20230919123045.tar
```
## Задание №9 
## Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.
## Решение
```
#!/bin/bash

# Проверка наличия двух аргументов
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <input_file> <output_file>"
    exit 1
fi

# Путь к входному и выходному файлам
input_file=$1
output_file=$2

# Проверка существования входного файла
if [ ! -f "$input_file" ]; then
    echo "Input file not found!"
    exit 1
fi

# Замена последовательностей из 4 пробелов на символ табуляции и запись в выходной файл
sed 's/    /\t/g' "$input_file" > "$output_file"

# Проверка успешности выполнения команды
if [ $? -eq 0 ]; then
    echo "Replacement complete. Output saved to: $output_file"
else
    echo "Failed to perform replacement."
    exit 1
fi
```



















