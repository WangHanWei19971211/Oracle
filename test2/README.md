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
