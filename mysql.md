#  MYSQL 操作语句 

## 表操作语句

* 创建表
  1. create table select 会将原表中的数据完整复制一份，但表结构中的索引会丢失。
  2. create table like 只会完整复制原表的建表语句，但不会复制数据
  3. Create table [if not exists] tbl_name （列定义） [表选项] 
* 查询表
  1. 查询数据库中存在的表 Show tables (from db_name) (like ‘pattern’);
  2. 查询当前表的定义语句 Show create table tbl_name;
  3. 查看当前表的列结构  Desc|describe tbl_name;(show columns from tbl_name);
* 删除表

​     Drop table [if exists] tbl_name,tbl_name; 可以删除多个表 多个表之间使用逗号分隔

* 更新表

  1. 重命名表  rename table old_tablename to new_tablename , old_tablename1 to new_tablename1;
  2. 更新表结构 alter tablename

     * 删除列   alter table [表名字] drop [列名称]   
     * 增加列  alter table [表名字] add [列名称] int not null comment '注释说明'
     * 修改列的类型信息  alter table [表名称] change [列名称]   【新列名称（这里可以和原来列同名即可）】bigint not null  comment '注释说明'
     * 重命名列  alter table [表名称] change [列名称] 【新列名称】 bigint not null  comment '注释说明'
     * 重命名表    alter table [表名称] rename [表新名称]
     * 删除表中主键   alter table [表名字] drop primary key
     * 添加主键  alter table [表名称] add constraint pk_s(约束的名称) primary key (id)
     * 添加索引 alter table [表名称] add index INDEX_NAME (name)
     * 删除索引 alter table [表名称] drop index index_name

* 获取数据

  1. 顺序

     * select 　× from table_name where group by  having order by limit

  2. 聚合函数 group_concat () 总结  

     * 简单理解 就是将相同的行合并起来
     * group_concat([DISTINCT] 要连接的字段 【Order BY ASC/DESC 排序字段】【Separator '分隔符']

  3. union联合

     * union用于把来自许多select 语句的结果组合到一个结果集合中
     * union可以对同一个表的两次查询联合起来. 这样做的益处也非常明显, 比如在blog应用中, 可以利用一条sql语句实现置顶blog和普通blog的分页显示.
     * union要求联合的两个表所要查找的数据列要一样多，如果一个表中没有另一个表的字段
     * union all 效率 高于 union

  4. 子查询

     * where型子查询 （把内层查询结果当做外层查询的比较条件）

        select goods_id,goods_name from goods where goods_id = (select max(goods_id) from goods);

     * from型子查询 （把内层的查询结果供外层再次查询）

          select name,avg(score) from stu where name in (select name from (select name,count(*) as gk from stu having gk >=2) as t) group by name;

     * exists型子查询 把外层查询结果拿到内层，看内层的查询是否成立

        select cat_id,cat_name from category where exists(select * from goods where goods.cat_id = category.cat_id);

  5. into outfile  导入导出数据

     * 使用select * into outfile 导出  SELECT * INTO OUTFILE 'd:\\test.txt' FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' LINES TERMINATED BY '\n' FROM test.table
     * 导入   LOAD DATA INFILE '/tmp/fi.txt' INTO TABLE test.fii FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' LINES TERMINATED BY '\n'
     * fields terminated by ',' 字段间分隔符
     * optionally enclosed by '"' 将字段包围对数值型无效
     * lines terminated by '\n' 换行符

  6. insert into 插入数据

     * 可以省略对列的指定，要求values()括号内，提供给了按照列顺序出现的所有字段的值或者使用set语法  insert into tbl_name set field = value,...;

     * 可以一次性使用多个值，采用(),(),();的形式 insert into tbl_name values (),(),();

     * 可以在列值指定时，使用表达式  insert into tbl_name values (field_value,10+10,now());

     * 可以使用一个填特殊值 default, 表示该列使用默认值  insert into tbl_name values (field_value,default)

     * 可以通过有个查询的结果，作为需要插入的值 insert into tbl_name select ...;

     * 可以指定在插入的值出现主键（或唯一索引）冲突时，更新其他非主键列的信息 insert into tbl_name 值 on duplicate key update 字段=值,...;   例子: 

       INSERT INTO TABLE (a,c) VALUES (1,3) ON DUPLICATE KEY UPDATE c=c+1;
       UPDATE TABLE SET c=c+1 WHERE a=1;

       ​

       ​

  ​

  ​

  ​	

   

  ​

  ​

  ​




​

​


