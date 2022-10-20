## Basic Syntax sql

### Installation of mysql in linux

```
root@ubuntu:/home/ubuntu# apt install mysql-server -y
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libaio1 libcgi-fast-perl libcgi-pm-perl libevent-core-2.1-7 libevent-pthreads-2.1-7 libfcgi-perl libhtml-template-perl libmecab2 mecab-ipadic mecab-ipadic-utf8 mecab-utils mysql-client-8.0
  mysql-client-core-8.0 mysql-server-8.0 mysql-server-core-8.0
Suggested packages:
  libipc-sharedcache-perl mailx tinyca
The following NEW packages will be installed:
  libaio1 libcgi-fast-perl libcgi-pm-perl libevent-core-2.1-7 libevent-pthreads-2.1-7 libfcgi-perl libhtml-template-perl libmecab2 mecab-ipadic mecab-ipadic-utf8 mecab-utils mysql-client-8.0
  mysql-client-core-8.0 mysql-server mysql-server-8.0 mysql-server-core-8.0
0 upgraded, 16 newly installed, 0 to remove and 518 not upgraded.
Need to get 31.7 MB of archives.
```

Secure installation mysql. it's not necessary to run this script if we just practicing inside our vm, but if we are in a company env it's very important to secure the mysql server.

```
root@ubuntu:/home/ubuntu# mysql_secure_installation 

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

Change the root password? [Y/n] n
 ... skipping.

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

Check the mysql service is running correctly.

```
root@ubuntu:/home/ubuntu# service mysql status
● mysql.service - MySQL Community Server
     Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2022-08-18 16:02:36 PDT; 4min 53s ago
    Process: 939 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=exited, status=0/SUCCESS)
   Main PID: 999 (mysqld)
     Status: "Server is operational"
      Tasks: 40 (limit: 4624)
     Memory: 424.3M
     CGroup: /system.slice/mysql.service
             └─999 /usr/sbin/mysqld

Aug 18 16:02:33 ubuntu systemd[1]: Starting MySQL Community Server...
Aug 18 16:02:36 ubuntu systemd[1]: Started MySQL Community Server.
```

Now if we want to run mysql if we set a password or we want to connect a remote server, we need to use this command.

```
# mysql -u root -h <remoteserver> -p <PORT NUMBER> -p <PASSWORD>
```

### Basics sql syntax and commands

Show the databases that are created on the system. ***note every sql commands or syntax it's need to be end with ";", if we don't specify that sql thinks that we are not done yet.***

```sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)
```

To create a database we need to use the following command.

```sql
mysql> create database test;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
5 rows in set (0.00 sec)
```

If we want to interact with a particular database or manipulate them we need to the following.

```sql
mysql> use databasename;
Database changed
mysql> 
```

Now a database have datables and columns that contain data but in this case it will tell us that is empty because we don't create any table in that database.

```sql
mysql> show tables;
Empty set (0.01 sec)

mysql>
```

To create a table we need to follow the following syntax,  first we specify to create a table with the ***create table*** syntax and then we open parentesis to add the fields inside of that table, now each field need to specify the datatype, what type of data is going to be store inside the of that field, for example, if we want to store just numbers we use the datatype ***int (intiger)*** which is a number, if is a text we use the datatype ***varchar***.

***Note if we add a comma on the last field and we close the parentesis it will output a syntax error in our sql syntax***.

```sql
CREATE TABLE _table_name_ (  
    _column1 datatype_,  
    _column2 datatype_,  
    _column3 datatype_  
);
```

Here i am creating a table with the follwing fields in oneliner code.

```sql
mysql> create table products ( id int, name varchar(255), region varchar(255), code int);
Query OK, 0 rows affected (0.00 sec)

mysql> show tables;
+----------------+
| Tables_in_test |
+----------------+
| products       |
+----------------+
1 row in set (0.00 sec)
```

Now once we created our table if we want to see the columns that we created inside that table we use ***desc or describe*** command. And we can see here the fields and the datatype.

***Note when we defined our tables or columns or fields that's part of defining our "Schema" or how are databases are arranged and organized.***

```sql
mysql> desc products;
+--------+--------------+------+-----+---------+-------+
| Field  | Type         | Null | Key | Default | Extra |
+--------+--------------+------+-----+---------+-------+
| id     | int          | YES  |     | NULL    |       |
| name   | varchar(255) | YES  |     | NULL    |       |
| region | varchar(255) | YES  |     | NULL    |       |
| code   | int          | YES  |     | NULL    |       |
+--------+--------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

Now let's insert rows in our table. Now here we use ***insert*** statement  to insert the row ***into*** the products tables, and with the ***values*** statement we indicate what data we are going to put inside of that row, now this works by order of the fields, so on the first we have id so i put the "1", and the second field is the name of the product so i put "laptop" and so on.

Now what is comfortable about sql that it's output if the out sql syntax it's correct or not, and it's output that 1 row is affected or modify ,in this case is the row that we add now.

```sql
mysql> insert into products values (1, "laptop", "madrid", "000123");
Query OK, 1 row affected (0.00 sec)
```

Now if we want to check the data that is store or take out the data inside our table we use ***select*** statement.

Basically here we indicate with ***select*** statement to select everything with the asterisk from the table products. This means that we want to output or take out all the data inside of the table.

```sql
mysql> select * from products;
+------+--------+--------+------+
| id   | name   | region | code |
+------+--------+--------+------+
|    1 | laptop | madrid |  123 |
+------+--------+--------+------+
1 row in set (0.00 sec)
```

If we want to just output a specific column we can use the follwing select statement. Here i output the data that contain on the column name from the table products.

```sql
mysql> select name from products;
+------------+
| name       |
+------------+
| laptop     |
| hard drive |
| keyboard   |
| GPU        |
+------------+
4 rows in set (0.00 sec)
```

We can use ***where*** statement to extract only those records thta we specify in our sql syntax. Here in this example i want to ***select*** everything using asterisk ***from*** the table "products", but i only want to extract ***where*** that contain "barcelona" on the region column.

```sql
mysql> select * from products where region = "barcelona";
+------+------------+-----------+---------+
| id   | name       | region    | code    |
+------+------------+-----------+---------+
|    2 | hard drive | barcelona |    1234 |
|    2 | CPU-intel  | barcelona | 1234567 |
+------+------------+-----------+---------+
2 rows in set (0.00 sec)
```

Let's say that we want to filter more information in that case we use ***or*** statement. In this example i want to extract rows that contain barcelona and madrid in the region column.

```sql
mysql> select * from products where region = "barcelona" or region = "madrid";
+------+------------+-----------+---------+
| id   | name       | region    | code    |
+------+------------+-----------+---------+
|    1 | laptop     | madrid    |     123 |
|    2 | hard drive | barcelona |    1234 |
|    2 | CPU-intel  | barcelona | 1234567 |
+------+------------+-----------+---------+
3 rows in set (0.00 sec)
```

In this case i have the following table called "Persons", in this case i extract names of the users that is less or equal (<=") than 30 years old using the where statement. 

```sql
mysql> select * from Persons;
+----------+------+---------+
| name     | age  | country |
+----------+------+---------+
| mark     |   30 | eeuu    |
| edwardo  |   25 | mexico  |
| mohammed |   35 | morocco |
| jose     |   15 | brazil  |
+----------+------+---------+
4 rows in set (0.01 sec)

mysql> select name from Persons where age <= 30;
+---------+
| name    |
+---------+
| mark    |
| edwardo |
| jose    |
+---------+
3 rows in set (0.00 sec)
```

or another example is that i want to output or extract inside the table "persons" the name of the persons that is less than 30 years old using where statement.

```sql
mysql> select name from Persons where age < 30;
+---------+
| name    |
+---------+
| edwardo |
| jose    |
+---------+
2 rows in set (0.00 sec)
```

We can combined ***where*** with ***AND***, ***OR*** and ***NOT*** operator.

+ The and operator display a record if all the conditions seperated by ***AND*** are true.
+ The ***OR*** operator display a record if any of the conditions seperated by ***OR*** is true.
+ The ***NOT*** operator displays a record if any of the conditions is NOT true.

Here in this example i filter the column "name" that ***NOT*** contain the word "barcelona" on the column "region" inside the table "Persons".

```sql
mysql> select * from products;
+------+------------+-----------+---------+
| id   | name       | region    | code    |
+------+------------+-----------+---------+
|    1 | laptop     | madrid    |     123 |
|    2 | hard drive | barcelona |    1234 |
|    2 | keyboard   | galicia   |    2345 |
|    2 | GPU        | roma      |   23456 |
|    2 | CPU-intel  | barcelona | 1234567 |
+------+------------+-----------+---------+
5 rows in set (0.00 sec)

mysql> select name from products where not region = "barcelona";
+----------+
| name     |
+----------+
| laptop   |
| keyboard |
| GPU      |
+----------+
3 rows in set (0.00 sec)
```

We can use ***delete*** statement to delete certain column or data inside our table. Here i am removing the row where contain the name "mark".

```sql
mysql> select * from Persons;
+----------+------+---------+
| name     | age  | country |
+----------+------+---------+
| mark     |   30 | eeuu    |
| edwardo  |   25 | mexico  |
| mohammed |   35 | morocco |
| jose     |   15 | brazil  |
+----------+------+---------+
4 rows in set (0.00 sec)

mysql> delete from Persons where name = "mark";
Query OK, 1 row affected (0.01 sec)

mysql> select * from Persons;
+----------+------+---------+
| name     | age  | country |
+----------+------+---------+
| edwardo  |   25 | mexico  |
| mohammed |   35 | morocco |
| jose     |   15 | brazil  |
+----------+------+---------+
3 rows in set (0.00 sec)
```

In the case that we want to modify a certain data inside our table we can use ***UPDATE*** statement to update or modify a certain column. In this case i want to change the country column to "france" ***where*** the name of the user is "edwardo". We use ***set*** to specify the cloumn inside the table persons that we want to modify.

```sql
mysql> update Persons set country = "France" where name = "edwardo";
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from Persons;
+----------+------+---------+
| name     | age  | country |
+----------+------+---------+
| edwardo  |   25 | France  |
| mohammed |   35 | morocco |
| jose     |   15 | brazil  |
+----------+------+---------+
3 rows in set (0.00 sec)
```

If in our ****update*** statement we don't use ***where*** statement it will apply our modification in all of the rows. Here in this case i don't use ***where*** statement so it will apply the word "france" in all the rows in the column country. so we need to be careful when we use the ***update*** statement.

```sql
mysql> select * from Persons;
+----------+------+---------+
| name     | age  | country |
+----------+------+---------+
| edwardo  |   25 | mexico  |
| mohammed |   35 | morocco |
| jose     |   15 | brazil  |
+----------+------+---------+
3 rows in set (0.00 sec)

mysql> update Persons set country = "France";
Query OK, 2 rows affected (0.00 sec)
Rows matched: 3  Changed: 2  Warnings: 0

mysql> select * from Persons;
+----------+------+---------+
| name     | age  | country |
+----------+------+---------+
| edwardo  |   25 | France  |
| mohammed |   35 | France  |
| jose     |   15 | France  |
+----------+------+---------+
3 rows in set (0.00 sec)
```

We can use ***ORDER BY*** keyword to sort the result-set in ascending ot descending order. Here i am using the ***order by*** to ascending  (asc) the age column of the users, so it will output by order the yongest to olders users inside the table.

```sql
mysql> select * from Persons;
+----------+------+---------+
| name     | age  | country |
+----------+------+---------+
| edwardo  |   25 | France  |
| mohammed |   35 | France  |
| jose     |   15 | France  |
+----------+------+---------+
3 rows in set (0.00 sec)

mysql> select * from Persons order by age asc;
+----------+------+---------+
| name     | age  | country |
+----------+------+---------+
| jose     |   15 | France  |
| edwardo  |   25 | France  |
| mohammed |   35 | France  |
+----------+------+---------+
3 rows in set (0.00 sec)
```

Same thing but descending (desc) the order of the column age. 

```sql
mysql> select * from Persons order by age desc;
+----------+------+---------+
| name     | age  | country |
+----------+------+---------+
| mohammed |   35 | France  |
| edwardo  |   25 | France  |
| jose     |   15 | France  |
+----------+------+---------+
3 rows in set (0.00 sec)
```

Remove a column inside a particular table, we can do that by using the ***alter table*** statement that is used to add, delete, or modify columns in a table, so in this case i use ***drop*** to remove the column "id".

```sql
mysql> alter table Persons drop column id;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> select * from Persons;
+----------+------+---------+
| name     | age  | country |
+----------+------+---------+
| mark     |   30 | eeuu    |
| edwardo  |   25 | mexico  |
| mohammed |   35 | morocco |
| jose     |   15 | brazil  |
+----------+------+---------+
4 rows in set (0.00 sec)
```

We can use ***alter table*** statement to add a column inside a existing table. Here i am adding the column "beard" inside the table "Persons" and i want to store in that column the datatype boolean, which is true or false in programming langueges.

```sql
mysql> alter table Persons add beard boolean;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> select * from Persons;
+----------+------+---------+-------+
| name     | age  | country | beard |
+----------+------+---------+-------+
| edwardo  |   25 | France  |  NULL |
| mohammed |   35 | France  |  NULL |
| jose     |   15 | France  |  NULL |
+----------+------+---------+-------+
3 rows in set (0.00 sec)
```

Let's say the user mohammed have beard, in this case using the ***update*** statement to ***set*** the boolean "True" ***where*** the name is "mohammend". So in this case "mohammed" have a beard is True (1).

```sql
mysql> update Persons set beard = True where name = "mohammed";
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from Persons;
+----------+------+---------+-------+
| name     | age  | country | beard |
+----------+------+---------+-------+
| edwardo  |   25 | France  |  NULL |
| mohammed |   35 | France  |     1 |
| jose     |   15 | France  |  NULL |
+----------+------+---------+-------+
3 rows in set (0.00 sec)
```

Same thing with the boolean "False".

```sql
mysql> update Persons set beard = False where name = "edwardo";
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from Persons;
+----------+------+---------+-------+
| name     | age  | country | beard |
+----------+------+---------+-------+
| edwardo  |   25 | France  |     0 |
| mohammed |   35 | France  |     1 |
| jose     |   15 | France  |  NULL |
+----------+------+---------+-------+
3 rows in set (0.00 sec)
```

If we want to delete a table we use the the "DROP TABLE" statement.

```
DROP TABLE tablename;
```

Samething to deleting a entire database using the statement ***Drop database***.

```
DROP DATABASE nameDB;
```

