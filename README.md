# SDB-12-08
Резервное копирование баз данных - Aleksandr Mihajlov SYS34  
  
### Задание 1. Резервное копирование

### Кейс
Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования. 

Необходимо описать, какие варианты резервного копирования подходят в случаях: 

1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.

1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.

1.3.* Возможен ли кейс, когда при поломке базы происходило моментальное переключение на работающую или починенную базу данных.

*Приведите ответ в свободной форме.*  
---  
Очень много факторов от которых зависит восстановление. Насколько развита сеть, размеры базы данных и какое время необходимо бэкапы держать.  

1.1 Я расмотрел бы два варианта полное восстановление или инкрементное.  
При полном бэкапе если БД большая, есть шанс не успеть выполнить бэкап. Но при этом варианте бэкап произойдет быстрее и это будет занимать много места.
При инкрементном долго будет выполняться только первый бэкап, следующий будет делать только изменения. Процесс восстановления будет дольше.  
  
1.2 В данном случае можно рассмотреть только инкрементный. Мы можем установить задачу делать бэкап каждый час, без нагрузки на сеть.
Бонусом будет множество точек восстановления.

1.3 Возможно самый простой способ это репликация.  
  
### Задание 2. PostgreSQL

2.1. С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).

2.1.* Возможно ли автоматизировать этот процесс? Если да, то как?

*Приведите ответ в свободной форме.*  
---  
2.1 pg_dump <параметры> <имя базы> > <файл, куда сохранить дамп>  
Пример  
```bash
pg_dump -U aleksandr -W pass > /tmp/users.dump
```  
-U имя пользователя для подключения к базе  
-W пароль для подключения к базе  
  
pg_restore <параметры соединения> <параметры> <имя файла>  
Пример  
```bash
pg_restore -d dbname /tmp/dbname.bak
```  
-d восстановление определенной БД  
  
### Задание 3. MySQL

3.1. С помощью официальной документации приведите пример команды инкрементного резервного копирования базы данных MySQL. 

3.1.* В каких случаях использование реплики будет давать преимущество по сравнению с обычным резервным копированием?

*Приведите ответ в свободной форме.*
---  
Инкрементное восстановление используется с ключем  --incremental или --incremental-with-redo-log-only для использования только журнала отката.  
Пример  
```bash
mysqlbackup --defaults-file=/home/dbadmin/my.cnf \
  --incremental --incremental-base=history:last_backup \
  --backup-dir=/home/dbadmin/temp_dir \
  --backup-image=incremental_image1.bi \
   backup-to-image
   ```