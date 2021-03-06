实验3：创建分区表

实验目的：
掌握分区表的创建方法，掌握各种分区方式的使用场景。


 创建orders表的部分语句是：

SQL> CREATE TABLE orders 

(

 order_id NUMBER(10, 0) NOT NULL 
 
 , customer_name VARCHAR2(40 BYTE) NOT NULL 
 
 , customer_tel VARCHAR2(40 BYTE) NOT NULL 
 
 , order_date DATE NOT NULL 
 
 , employee_id NUMBER(6, 0) NOT NULL 
 
 , discount NUMBER(8, 2) DEFAULT 0 
 
 , trade_receivable NUMBER(8, 2) DEFAULT 0 
 
) 

TABLESPACE USERS 

PCTFREE 10 INITRANS 1 

STORAGE (   BUFFER_POOL DEFAULT ) 

NOCOMPRESS NOPARALLEL 

PARTITION BY RANGE (order_date) 

(

 PARTITION PARTITION_BEFORE_2016 VALUES LESS THAN (
 
 TO_DATE(' 2016-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS',
 
 'NLS_CALENDAR=GREGORIAN')) 
 
 NOLOGGING 
 
 TABLESPACE USERS 
 
 PCTFREE 10 
 
 INITRANS 1 
 
 STORAGE 
 
( 

 INITIAL 8388608 
 
 NEXT 1048576 
 
 MINEXTENTS 1 
 
 MAXEXTENTS UNLIMITED 
 
 BUFFER_POOL DEFAULT 
 
) 

NOCOMPRESS NO INMEMORY  

, PARTITION PARTITION_BEFORE_2017 VALUES LESS THAN (

TO_DATE(' 2017-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS',

'NLS_CALENDAR=GREGORIAN')) 

NOLOGGING 

TABLESPACE USERS02 

);

![第一步](https://github.com/WangHanWei19971211/Oracle/blob/master/test3/order.png)


创建order_details表的部分语句如下：

SQL> CREATE TABLE order_details 

(

id NUMBER(10, 0) NOT NULL 

, order_id NUMBER(10, 0) NOT NULL

, product_id VARCHAR2(40 BYTE) NOT NULL 

, product_num NUMBER(8, 2) NOT NULL 

, product_price NUMBER(8, 2) NOT NULL 

, CONSTRAINT order_details_fk1 FOREIGN KEY  (order_id)

REFERENCES orders  (  order_id   )

ENABLE 

) 

TABLESPACE USERS 

PCTFREE 10 INITRANS 1 

STORAGE (   BUFFER_POOL DEFAULT ) 

NOCOMPRESS NOPARALLEL

PARTITION BY REFERENCE (order_details_fk1)

(

PARTITION PARTITION_BEFORE_2016 

NOLOGGING 

TABLESPACE USERS 

) 

NOCOMPRESS NO INMEMORY, 

PARTITION PARTITION_BEFORE_2017 

NOLOGGING 

TABLESPACE USERS02

) 

NOCOMPRESS NO INMEMORY  

);

![第二步](https://github.com/WangHanWei19971211/Oracle/blob/master/test3/orders1.png)


查看表空间的数据库文件，以及每个文件的磁盘占用情况:

![3](https://github.com/WangHanWei19971211/Oracle/blob/master/test3/select.png)
