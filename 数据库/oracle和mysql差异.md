1，在mybatis框架里，在xml里面写新增或者更新的脚本的时候，如果是后台过来的参数是空的话，如果是oracle，则直接报错，如果是mysql，则默认赋一个空值进去；

2.，在mybatis框架里，在xml里面写查询脚本的时候，如果后面有分号，如果是oracle，则直接报错，如果是mysql，则没有上什么问题；

3，在写关于时间的脚本的时候，mybatis会自动把字符串的时间转化为date类型，例如：create_time >  '2010-02-03'，这个时间字符串会自动转化；但是oracle就不会，一定要create_time >  to_date('2010-02-03','yyyy-MM-dd')，具体的函数进行自动转化；

4，关于多表的连接查询：

MySQL可以这样，也是我最喜欢的 　

```sql
SELECT pdba.*
FROM TSE_AGREEMENT_INFO_NEW ain
INNER JOIN TSE_SAPFI_SUPPLIER tss 
INNER JOIN pmp_agnt_sign_info asi 
INNER JOIN pmp_agnt_mark_info ami 
INNER JOIN PMP_DIFFI_BIDDING_AREAS pdba
ON tss.lifnr = ain.supplier_id 
      AND tss.user_id = asi.user_id
      AND asi.mark_code = ami.mark_code AND asi.mark_batch = asi.mark_batch
      AND ami.cover_id = pdba.cover_id
WHERE ain.agreemen_id = '10001129321429';
```

ORACLE 只能这样，而且MySQL也可以这样好像

```sql
SELECT pdba.*
FROM TSE_AGREEMENT_INFO_NEW ain, 
      TSE_SAPFI_SUPPLIER tss, 
      pmp_agnt_sign_info asi, 
      pmp_agnt_mark_info ami, 
      PMP_DIFFI_BIDDING_AREAS pdba
WHERE tss.lifnr = ain.supplier_id 
  AND tss.user_id = asi.user_id
  AND asi.mark_code = ami.mark_code AND asi.mark_batch = asi.mark_batch
  AND ami.cover_id = pdba.cover_id
  AND ain.agreemen_id = '10001129321429';
```

5，mysql 可以一次性插入多条数据，例如 insert into tableA (col1,col2) values(val1,val2),(val3,val4)，而oracle不可以；

6，mysql 可以 SELECT t.empno,t.job,COUNT(*) FROM emp t GROUP BY t.job，而oracle不可以；
7，mysql 可以 SELECT t.deptno,SUM(t.sal) AS cnt FROM emp t GROUP BY t.deptno HAVING cnt > 12000，
  oracle 一定要 SELECT t.deptno,SUM(t.sal) FROM emp t GROUP BY t.deptno HAVING SUM(t.sal) > 12000

8，MySQL建立视图不能用子查询（解决的方法就是多个视图关联查询之后再新视图），Oracle建立视图可以用子查询；