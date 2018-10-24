实验2：用户管理 - 掌握管理角色、权根、用户的能力，并在用户之间共享对象。


第1步：以system登录到pdborcl，创建角色con_res_wang和用户new_wang，并授权和分配空间：

$ sqlplus system/123@pdborcl

创建角色con_res_wang：

SQL> CREATE ROLE con_res_wang;

给con_res_wang分配空间：

SQL> GRANT connect,resource,CREATE VIEW TO con_res_wang;

创建角色new_wang：

SQL> CREATE USER new_wang IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;

授权new_wang访问users表空间，空间限额是50M

SQL> ALTER USER new_wang QUOTA 50M ON users;

SQL> GRANT con_res_wang TO new_wang;

![第一步](https://github.com/WangHanWei19971211/Oracle/blob/master/test2/1.png)



第2步：新用户new_wang连接到pdborcl，创建表mytable和视图myview，插入数据，最后将myview的SELECT对象权限授予hr用户。

$ sqlplus new_wang/123@pdborcl

SQL> show user;

USER is "NEW_WANG"

SQL> CREATE TABLE mytable (id number,name varchar(50));

Table created.

SQL> INSERT INTO mytable(id,name)VALUES(1,'zhang');

1 row created.

SQL> INSERT INTO mytable(id,name)VALUES (2,'wang');

1 row created.

SQL> CREATE VIEW myview AS SELECT name FROM mytable;

View created.

SQL> SELECT * FROM myview;

NAME

--------------------------------------------------

zhang

wang

SQL> GRANT SELECT ON myview TO hr;

Grant succeeded.

SQL>exit

![第二步](https://github.com/WangHanWei19971211/Oracle/blob/master/test2/2.png)


第3步：用户hr连接到pdborcl，查询new_wang授予它的视图myview

$ sqlplus hr/123@pdborcl

SQL> SELECT * FROM new_wang.myview;

NAME

--------------------------------------------------

zhang

wang

SQL> exit

![第三步](https://github.com/WangHanWei19971211/Oracle/blob/master/test2/3.png)


查看数据库的使用情况：

![第四步](https://github.com/WangHanWei19971211/Oracle/blob/master/test2/4.png)

