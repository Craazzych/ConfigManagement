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
```[root@localhost etc]# cat /etc/protocols ...
142 rohc
141 wesp
140 shim6
139 hip
138 manet
```
## Решение
``` awk '{print $3, $1}' /etc/protocols | sort -nr | head -n 5```
```protocols,v #
for #
XTP xtp
XNS-IDP xns-idp
VMTP vmtp```
