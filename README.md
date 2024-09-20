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
