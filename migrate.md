##总结表分割的方法

###将city.sql的数据进行分割成 ID 省 市 县 的形式
####声明第一，第二种分割方式是差不多的
1. 通过备份表的形式进行表分割
>这个办法就比较方便，但。。。当大数据来的时候就会显得效率慢，性能低

`create table city_all as
 select s2.id id, s.cityName province,s2.cityName city, null district from s_provinces s join s_provinces s2 on s.id = s2.parentId where s2.depth =2
 union
 select s3.id, s.cityName,s2.cityName ,s3.cityName from s_provinces s join s_provinces s2 join s_provinces s3 on s.id = s2.parentId and s2.id = s3.parentId where s2.depth >1;
`

>这个仅仅是三千多条数据，如果真的是达到E级以上的数据量，那就麻烦了
2. 通过在原表的基础上进行修改分割
>思路:  
>1 建表  
>2 通过原表查出你要的数据  
>3 然后通过插入表的方式把数据导进表中    

以上是我们今天的一个小任务的一个总结
#
###其实分割的方法是千变万化的,但要相信一点(万变不离其宗)我们的一些方法都是从最基础的东西演变而来
####比如一些优化方案:
* 通过使用存储过程进行表的分割
* 通过在客户端的表的处理
* 
#
###题外话：
* 分区的方法。。。具体还在研究  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.1 RANGE 分区 :  
>先秀一下语法:        
`create table if not exists city (
   id int
 ) partition by range (id)
 (partition  p1 values less than (10000),
 partition p2 values less than maxvalue );`  
 解释一波: 我们创建一个只有一个ID的表，如果ID是1>>10000就可以写入, 如果不写最后一句代码将会造成大于10000就不可以插入了  
 然后我们将1>>10000的数据存储在 p1 这个区中    
 
###论:
* 为什么要分表和分区
>分表和表分区的目的就是减少数据库的负担，提高数据库的效率，通常点来讲就是提高表的增删改查效率  
* 分表与分区的缘分
>1）、都能提高mysql的性能，在高并发状态下都有一个良好的表现  
>2）、分表和分区不矛盾，可以相互配合的，对于那些大访问量，并且表数据比较多的表，我们可以采取分表和分区结合的方式，访问量不大，但是表数据很多的表，我们可以采取分区的方式等   
>3）、分表技术是比较麻烦的，需要手动去创建子表，app服务端读写时候需要计算子表名。采用merge好一些，但也要创建子表和配置子表间的union关系  
>4）、表分区相对于分表，操作方便，不需要创建子表  

具体还是在研究...