# Практическое занятие №1. Введение, основы работы в командной строке

Научиться выполнять простые действия с файлами и каталогами в Linux из командной строки. Сравнить работу в командной строке Windows и Linux.

## Задача 1
Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd (вам понадобится grep).
```
cat /etc/passwd | cut -d ":" -f 1 | sort
```
![image](https://github.com/user-attachments/assets/3e9590af-dfc4-4a32-8c79-e4b2986863fc)


## Задача 2
Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов.
```
cat /etc/protocols | awk '{print $2, $1}' | sort -nr | head -n 5
```
![image](https://github.com/user-attachments/assets/dee9c942-058b-4b97-ad31-639cae41083e)


## Задача 3
Написать программу banner средствами bash для вывода текстов, как в следующем примере (размер баннера должен меняться!)
```
#!/bin/bash
string="| "
string+=$1
string+=" |"
length=${#string}
str1="+"
for (( i=0; i < length - 2; i++ ))
do
str1+="-"
done
str1+="+"
echo $str1
echo "$string"
echo $str1
```
![image](https://github.com/user-attachments/assets/3b326912-4b40-4647-9db8-2f483faa94b0)


## Задача 4
Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).
```
#!/bin/bash
cat "$1" | tr -c "abcdefghijklmnopqrstuvwxyz" " " | xargs | tr " " "\n" | sort -u | xargs
```
![image](https://github.com/user-attachments/assets/042fb96b-0c5d-4f57-bef2-49eeacfe57e5)


## Задача 5
Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin). Например, пусть программа называется reg. В результате для banner задаются правильные права доступа и сам banner копируется в /usr/local/bin.
```
#!/bin/bash
touch "$1"
chmod +x "$1" 
cp "$1" /usr/local/bin
```
![image](https://github.com/user-attachments/assets/07f48a14-d3b7-4b3d-8e9a-d03d2d6cb2dc)



## Задача 6
Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.
```
#!/bin/bash
if [[ "$1" =~ \.c$|\.js$|\.py$ ]]; then
    first_line=$(head -n 1 "$1")
    if [[ "$1" =~ \.c$|\.js$ ]]; then
        if [[ "$first_line" =~ ^[[:space:]]*// ]] || [[ "$first_line" =~ ^[[:space:]]*/\* ]]; then
            echo "Комментарий найден в $1"
        else
            echo "Комментарий отсутствует в $1"
        fi
    elif [[ "$1" =~ \.py$ ]]; then
        if [[ "$first_line" =~ ^[[:space:]]*# ]]; then
            echo "Комментарий найден в $1"
        else
            echo "Комментарий отсутствует в $1"
        fi
    fi
else
    echo "Файл должен иметь расширение .c, .js или .py"
fi
```
![image](https://github.com/user-attachments/assets/6f95492a-d47e-467e-bc3b-2b0649a9b57c)



## Задача 7
Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).
```
#!/bin/bash
find "$1" -type f | while read -r file; do
    sha256sum "$file"
done | sort | uniq -w64 -d
```
![image](https://github.com/user-attachments/assets/5c8f676f-5b08-4c41-b777-3c586caf53ac)



## Задача 8
Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.
```
#!/bin/bash
dir="$1"
ext="$2"
archive="archive.tar"
find "$dir" -type f -name "*$ext" | tar -cf "$archive" -T -
```
![image](https://github.com/user-attachments/assets/78a13079-524c-43f3-8a5f-55b122da1ecb)



## Задача 9
Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.
```
#!/bin/bash
sed 's/    /\t/g' "$1" > "$2"
```
![image](https://github.com/user-attachments/assets/ee6e0177-ff3d-4736-81cb-8e7922c93a35)


## Задача 10
Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром.
```
#!/bin/bash
find "$1" -type f -name "*.txt" -exec test ! -s {} \; -print
```
![image](https://github.com/user-attachments/assets/08e77489-9c10-4ebb-99da-1b7e376f0157)
