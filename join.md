##类似内联数据表的数据查询
####表中的一些数据如图
![表](file:///D:/git/data.jpg)
***
需求--查询出所有广东省的市、县
***
* 使用内连接进行查询

` 
select shi.id ID , xian.cityName province  , sheng.cityName city ,shi.cityName district  from s_provinces sheng ,s_provinces shi ,s_provinces xian where sheng.id = shi.parentId and sheng.parentId = xian.id and xian.cityName = '广东省'
  union
  select shi.id , xian.cityName , sheng.cityName ,null  from s_provinces sheng join s_provinces shi join s_provinces xian on sheng.id = shi.parentId and sheng.parentId = xian.id where xian.cityName = '广东省'`
通过三个表内连接的方式把数据关联起来，然后查出是广东省的，然后其中有一条数据在进行第一个虚拟表时就被排除出去了，因为他的一个要关联的字段为空，

*也有使用多表基本连接查询（其实这个原理是和内连接无异，然后我们建议使用内连接的方式进行查询）

`select d.id, d.cityName ,s.cityName ,w.cityName from s_provinces s ,s_provinces d ,s_provinces w where s.id = d.parentId and s.parentId = w.id and w.cityName= '广东省' ;
`

###--分析--
我们使用这种建表方式是否合适？
>合适这个东西，要看需求 ，这种表查询比较方便，但进行数据修改，添加，删除时就会麻烦
