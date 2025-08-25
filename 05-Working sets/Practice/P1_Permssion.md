### Tags:[[Other]]
## Задания[](https://basis.gnulinux.pro/ru/latest/basis/20/20._%D0%9F%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D0%BA%D0%B0.html#id3 "Ссылка на этот заголовок")

1. Создайте группы company, it, sales Создайте директории /company, /company/it, /company/sales, /company/shared Создайте пользователей ituser1, ituser2, itadmin, suser1, suser2, sadmin так, чтобы у всех пользователей основной группой должна быть company у пользователей it* оболочкой должен быть bash, дополнительной группой должна быть it, домашняя директория должна находиться внутри директории /company/it/ у пользователей s* оболочкой должен быть sh, дополнительной группой должна быть sales, домашняя директория внутри /company/sales Содержимое директории /company/sales должны видеть только root и участники группы sales  
    Содержимое директории /company/it должны видеть только root и участники группы it Все вышеуказанные пользователи должны видеть содержимое директории /company/shared При этом каждый пользователь лично должен
    внутри директории /company/shared создать файл со своим логином.
2. Создайте группы academy, teachers и students, соответствующие директории, а также пользователей (student & teacher) У всех пользователей должны быть домашние директории в соответствующих директориях (/academy/students/student). Только пользователи групп должны иметь доступ на соответствующие директории (/academy/students). Также создайте группу staff, у которой будет доступ во все указанные директории Создайте расшаренную директорию /academy/secret, внутри которой пользователи групп teachers и staff смогут создавать файлы, но не смогут удалять чужие файлы. У пользователей группы students не должно быть доступа. Создайте «программу» spy, с помощью которой студенты смогут смотреть содержимое директории /academy/secret[1](https://basis.gnulinux.pro/ru/latest/basis/20/20._%D0%9F%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D0%BA%D0%B0.html#id5). И с помощью sudo разрешите пользователям группы students от имени группы teachers удалять файлы внутри /academy/secret Создайте файл от учителя внутри директории /academy/secret, найдите имя файла с помощью spy от имени студента и удалите файл с помощью sudo от имени студента.
3. Права: Создать директорию /home/company. Внутри этой директории нужно создать директории homedirs, sales, marketing, shared. Создать группы company, sales и marketing, а также пользователей mike (основная группа - sales, дополнительная - company) и john (основная группа - marketing, дополнительная - company). У пользователей домашнии директории должны находиться в директории /home/company/homedirs. Директории /home/company/sales и /home/company/marketing должны принадлежать соответствующим группам. У пользователя и группы должны быть все права на директорию. Директория shared должна быть расшаренной, её группа должна быть company, и чтобы все новые файлы в этой директории принадлежали группе company.
4. Создайте 3 группы и директории внутри /data – marketing, sales и it и 4 пользователя – user.marketing, user.sales, admin и backup. Владельцем директорий должны быть соответствующие пользователи – user.marketing владеет директорией /data/marketing и т.д. Группы директорий должны соответствовать названиям. У остальных пользователей не должно быть доступа к директориям. Пользователь admin должен иметь полный доступ ко всем директориям, пользователь backup должен иметь доступ только на чтение.
5. Создайте директорию /company, чтобы владельцем был root, а группой – company. У владельца и группы должны быть все права, у остальных никаких. Пользователи не должны иметь возможность удалять чужие файлы в этой директории, а все файлы, созданные в этой директории, должны иметь группу company.

## Comands and result
1.
```
root@gleck:/home/company# useradd -b /home/company/it -g 10000 -s /bin/bash ituser1
root@gleck:/home/company# passwd ituser1 
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: password updated successfully
root@gleck:/home/company# su - ituser1 
ituser1@gleck:~$ pwd
/home/company/it/ituser1
ituser1@gleck:~$ 
~~~~~~~~~~~~~~~~~~~~~~~~
root@gleck:/home/company# usermod -aG company,IT -g company ituser2
root@gleck:/home/company# id ituser2
uid=10203(ituser2) gid=10053(company) groups=10053(company),10000(IT)
~~~~~~~~~~~~~~~~~~~~~~~~
root@gleck:/home/company# useradd -b /home/company/it -g company -G company,IT -s /bin/bash itadmin
root@gleck:/home/company# id itadmin 
uid=10204(itadmin) gid=10053(company) groups=10053(company),10000(IT)
~~~~~~~~~~~~~~~~~~~~~~~~
root@gleck:/home/company# usermod -aG sails,company -g company -s /bin/sh sadmin
~~~~~~~~~~~~~~~~~~~~~~~~
root@gleck:/home/company# chown itadmin:IT ./it
root@gleck:/home/company# chown sadmin:sails ./sales
root@gleck:/home/company# ls -l
total 8
drwxr-xr-x 5 itadmin IT    4096 Aug 11 19:53 it
drwxr-xr-x 5 sadmin  sails 4096 Aug 11 20:11 sales
~~~~~~~~~~~~~~~~~~~~~~~~
root@gleck:/home/company# chmod 3770 it
root@gleck:/home/company# chmod 3770 sales
root@gleck:/home/company# ls -l
total 8
drwxrws--T 5 itadmin IT    4096 Aug 11 19:53 it
drwxrws--T 5 sadmin  sails 4096 Aug 11 20:11 sales

```
