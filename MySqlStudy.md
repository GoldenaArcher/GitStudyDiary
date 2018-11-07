# MySql Study Diary

# Table of Contents
- [Install MySql](#Install-MySql)
- [Command](#Command)
	- [Start or Terminate MySql](#Start-or-Terminate-MySql)
	- [Parameters](#Parameters)
	- [MySql Commands](#MySql-Commands)
		- [Exit Database](#Exit-Database)
		- [Change Prompt](#Change-Prompt)
		- [Common Commands](#Common-Commands)
		- [Database Operations](#Database-Operations)
- [Data Type](#Data-Type)


## Install MySql <a name="Install-MySql">
There are plenty of ways to install MySql on different platform. Since I've already installed XAMPP, so I just keep XAMPP running and use MySql installed 
by XAMPP. 

With Windows user, there is a MSI file provided by MySql which can be easily downloaded. For Linux and Mac OS user, download MySql from command line is also 
pretty easy.

By default, the _port_ of MySql should be _3306_, and the default character set is utf8. How to change it is also easy. If GUI is preferred, MySql Workbench 
is the best friend, all the changes can be easily modified on the Workbench.

## Command <a name="Command">

### Start or Terminate MySql<a name="Start-or-Terminate-MySql">

On Windows:

`net start mysql` to start, and `net stop mysql` to stop the service. If MySql is installed with XAMPP, the command line doew not work.

### Parameters <a name="Parameters">
- -D, --database=name
- -h, --host-name, if not given, by default is `127.0.0.1`

	`$ mysql -h127.0.0.1`

- -p, --password[=name], in the 1st example, the password is root; in the second example, password is 
encrypted.

	`$ mysql -proot` 
	
	__or__
	```		
	$mysql -p
	Enter password: ******
	```

- -P, --port=#, if not given, by default, is `3306`

	`$ mysql -P3306`

- -u, --user=name, in the below example, username is root

	`$ mysql -uroot`

- -V, --version

	```
	$ mysql -V
	mysql  Ver 15.1 Distrib 10.1.35-MariaDB, for Win32 (AMD64)
	```
	
### MySql Commands <a name="MySql-Commands">

Once entered the db, below commands are necessary to learn in order to use database in command line promp.

#### Exit Database <a name="Exit-Database">
```
$ mysql > exit;
$ mysql > quit;
$ mysql > \q;
```
	
#### Change Prompt <a name="Change-Prompt">

Generally speaking, there are two ways change the prompt, one is before connecting the database, the other 
is after connected to the database.

Here are some commonly used parameters for prompt:

| Parameters | Description | Representation |
| :--------: |:-----------:|:-------------: |
| \D | Current Date | Wed Nov  7 05:35:48 2018 |
| \d | Current database | (none) if not selected, changed after selecting db |
| \h | Current server name | localhost |
| \u | Current user | root |

- Before Connecting to DB:
	```
	$ mysql -uroot -proot --prompt \h
	localhost
	```

- After connected to DB:
	```
	$ mysql -uroot -proot 
	$ MariaDB [(none)]>  prompt mysql >
	PROMPT set to 'mysql >'
	
	$ mysql >prompt \u@\h \d> 
	PROMPT set to '\u@\h \d> '
	
	$ root@localhost (none)> use mysql;
	Database changed
	
	$ root@localhost mysql> 
	```

#### Common Commands <a name="Common-Commands">

Even though it is recommended to use captical letters for keywords and arguments, lower case letters are also recognized.
However, sql commands must end with semicolon `;`.

- Display current version:
	```
	$ (none) > select version();
	+-----------------+
	| version()       |
	+-----------------+
	| 10.1.35-MariaDB |
	+-----------------+
	1 row in set (0.00 sec)
	```

- Display current user:

	```
	$ (none) > select user();
	+----------------+
	| user()         |
	+----------------+
	| root@localhost |
	+----------------+
	1 row in set (0.00 sec)
	```

- Display current time:

	```
	$ (none) > select now();
	+---------------------+
	| now()               |
	+---------------------+
	| 2018-11-07 05:49:26 |
	+---------------------+
	1 row in set (0.00 sec)
	```

#### Database Operations <a name="Database-Operations">

- Create Database

	Grammar: CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARSET SET [=] charset_name

	`{}` contains the necessary/must command, `[]` represents the optional commands.

	```
	# Most simple command
	$ (none) > CREATE DATABASE t1;
	Query OK, 1 row affected (0.01 sec)

	# If database has already been created
	$ (none) > CREATE DATABASE t1;
	ERROR 1007 (HY000): Can't create database 't1'; database exists

	$ (none) > CREATE DATABASE IF NOT EXISTS t1;
	Query OK, 0 rows affected, 1 warning (0.00 sec)

	$ (none) > SHOW WARNINGS;
	+-------+------+---------------------------------------------+
	| Level | Code | Message                                     |
	+-------+------+---------------------------------------------+
	| Note  | 1007 | Can't create database 't1'; database exists |
	+-------+------+---------------------------------------------+
	1 row in set (0.00 sec)

	$ (none) > CREATE OR REPLACE DATABASE t1;
	Query OK, 2 rows affected (0.03 sec)

	#Check the command to create database
	$ (none) > SHOW CREATE DATABASE t1;
	+----------+---------------------------------------------------------------+
	| Database | Create Database                                               |
	+----------+---------------------------------------------------------------+
	| t1       | CREATE DATABASE `t1` /*!40100 DEFAULT CHARACTER SET latin1 */ |
	+----------+---------------------------------------------------------------+
	1 row in set (0.00 sec)

	# create database in other charset, i.e., GBK

	$ (none) > drop database t1;
	Query OK, 0 rows affected (0.00 sec)

	$ (none) > CREATE DATABASE IF NOT EXISTS t1 CHARACTER SET GBK;
	Query OK, 1 row affected (0.00 sec)

	$ (none) > SHOW CREATE DATABASE t1;
	+----------+------------------------------------------------------------+
	| Database | Create Database                                            |
	+----------+------------------------------------------------------------+
	| t1       | CREATE DATABASE `t1` /*!40100 DEFAULT CHARACTER SET gbk */ |
	+----------+------------------------------------------------------------+
	1 row in set (0.00 sec)
	```

- Check Database

	Grammar: SHOW {DATABASE | SCHEMA} [LIKE 'pattern' | WHERE expression]

	```
	$ (none) > SHOW DATABASES;
	+--------------------+
	| Database           |
	+--------------------+
	| ...                |
	| t1                 |
	| ...                |
	+--------------------+
	10 rows in set (0.00 sec)
	```

- Change/Update database

	- Change/Update charset
	
		Grammar: ALTER {DATABASE | SCHEMA} [db_name] [DEFAULT] CHARACTER SET = charset_name;
		
		```
		# already create a database t1 which charset is GBK
		$ (none) > SHOW CREATE DATABASE t1;
		+----------+------------------------------------------------------------+
		| Database | Create Database                                            |
		+----------+------------------------------------------------------------+
		| t1       | CREATE DATABASE `t1` /*!40100 DEFAULT CHARACTER SET gbk */ |
		+----------+------------------------------------------------------------+
		1 row in set (0.00 sec)
		
		$ (none) > ALTER DATABASE t1 CHARACTER SET utf8;
		Query OK, 1 row affected (0.00 sec)

		$ (none) > SHOW CREATE DATABASE t1;
		+----------+-------------------------------------------------------------+
		| Database | Create Database                                             |
		+----------+-------------------------------------------------------------+
		| t1       | CREATE DATABASE `t1` /*!40100 DEFAULT CHARACTER SET utf8 */ |
		+----------+-------------------------------------------------------------+
		1 row in set (0.00 sec)
		```
		
- Delete Database
	
	Grammar: DROP {DATABASE | SCHEMA} [IF EXISTS] db_name;
	
	```
	$ (none) > DROP DATABASE t1;
	Query OK, 0 rows affected (0.00 sec)
	
	# At this moment, there is no t1 exists in system
	
	$ (none) > DROP DATABASE t1;
	ERROR 1008 (HY000): Can't drop database 't1'; database doesn't exist
	
	$ (none) > DROP DATABASE IF EXISTS t1;
	Query OK, 0 rows affected, 1 warning (0.00 sec)
	```
	
## Data Type <a name="Data-Type">

- Integer Type

	| Data Type | Data Range | Data Size |
	| :-------: | :--------: | :-------: |
	| __TINYINT__ | signed: -128 - 127(-2<sup>7</sup> - 2<sup>7</sup>-1)<br>unsigned: 0 - 255 (0 - 2<sup>8</sup>-1) | 1 byte|
	| __SMALLINT__ | signed: -2<sup>15</sup> - 2<sup>15</sup>-1<br>unsigned: 0 - 2<sup>16</sup>-1 | 2 bytes |
	| __MEDIUMINT__ | signed: -2<sup>23</sup> - 2<sup>23</sup>-1<br>unsigned: 0 - 2<sup>24</sup>-1 | 3 bytes |
	| __INT__ | signed: -2<sup>31</sup> - 2<sup>31</sup>-1<br>unsigned: 0 - 2<sup>32</sup>-1 | 4 bytes |
	| __BIGINT__ | signed: -2<sup>63</sup> - 2<sup>63</sup>-1<br>unsigned: 0 - 2<sup>64</sup>-1 | 8 bytes |
	
- Floating Number

	| Data Type | Data Range | Example |
	| :-------: | :--------: | :-----: |
	| __Float__(_M_, _D_) | 4 bytes<br>-3.402823466E+38 to -1.175494351E-38<br>and<br>1.175494351E-38 to 3.402823466E+38 | FLOAT(7,4) will look like -999.9999 in DB |