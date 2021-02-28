### 空值

注意 = NULL 还是 IS NULL

```sql
SELECT * FROM EMP T WHERE T.COMM IS NULL;
SELECT * FROM EMP T WHERE T.COMM IS NOT NULL;
UPDATE EMP SET COMM = NULL;
```

### NOT

```sql
SELECT * FROM EMP T WHERE T.JOB != 'CLERK'  AND SAL > 2000; -- 这个简单
SELECT * FROM EMP T WHERE NOT (T.JOB = 'CLERK' OR SAL <= 2000); -- 这个之前掉过坑，not 在between and 之前也是可以的，我记得
```

### 字符串处理

```sql
SELECT * FROM EMP T WHERE T.ENAME LIKE '_M%'; -- 查询第2个字母是M的全部雇员的信息('_' 匹配单个任意字符，常用来限制表达式的字符长度)
SELECT UPPER('aaa'), LOWER('BBB') FROM DUAL; -- 字符串大小写转换
SELECT INITCAP('abc') FROM DUAL; -- 句首转大写，输出 Abc
SELECT REPLACE('ABCD', 'A', 'a') FROM DUAL; -- 替换，输出 aBCD
SELECT LENGTH('aaaaa') FROM DUAL; -- 求字符串长度
SELECT SUBSTR('abcd', 1, 2) FROM DUAL; -- 输出 ab,其中 select substr('abcd',1,2) FROM dual; 效果是一样的，oracle很灵活，从0到1开始都可以
SELECT T.ENAME, SUBSTR(T.ENAME, LENGTH(T.ENAME) - 2) FROM EMP T; -- 截取后3个字母
SELECT LTRIM(' aa  '), RTRIM(' aa  '), TRIM(' aa  ') FROM DUAL; -- 去掉左边，右边，左右两边空格
SELECT LPAD('aaa', 5, '*'), RPAD('aaa', 5, '*') FROM DUAL; -- 填充函数，分别输出：**aaa   aaa**
SELECT INSTR('aaaaa', 'a') FROM DUAL; -- 字符串查找函数，可以查抄得到返回1（表示是），查找不到返回0（表示否）
SELECT '编号是' || T.EMPNO || '哈哈' FROM EMP T; -- 字符连接
SELECT CONCAT('ddd', T.EMPNO) FROM EMP T; -- 字符连接
```

### 数值处理

```sql
SELECT ROUND(111.44486, 3) FROM DUAL; -- 输出111.445；对小数进行四舍五入，可以指定保留位数；如果不指定，则是对小数点之后的数字全部全部四舍五入
SELECT TRUNC(111.44486, 3) FROM DUAL; -- 输出111.444；截取字符串
SELECT ROUND(789.625, -2) FROM DUAL; -- 对比，明显trunc 也可以
SELECT ROUND(789.675, 2) FROM DUAL; -- 对比，明显trunc 也可以
SELECT MOD(10, 4) FROM DUAL; -- 输出：2；取模
```

### 日期处理

```sql
SELECT SYSDATE + 3, SYSDATE - 3 FROM DUAL; -- 当前时间加上一天，减去一天
SELECT SYSDATE - T.HIREDATE FROM EMP T; -- 计算出以天数为单位的数值
SELECT SYSDATE, ADD_MONTHS(SYSDATE, 3), ADD_MONTHS(SYSDATE, -3) FROM DUAL; -- 三个月之前，之后的日期（注意：三个月之后以及90天之后的概念是不一致的）
SELECT SYSDATE, NEXT_DAY(SYSDATE, '星期三') FROM DUAL; -- 求出下一个星期三的日期
SELECT SYSDATE, LAST_DAY(SYSDATE) FROM DUAL; -- 求出当月最后一天的日期
SELECT MONTHS_BETWEEN(SYSDATE, HIREDATE) FROM EMP; -- 当前时间与雇佣时间相差的月数（注意，有小数点的，所以要想取整，要trunc(months_between(SYSDATE,hiredate))）,所以，要求年数的话再除以12;
SELECT EXTRACT(YEAR FROM SYSDATE) AS YEARS,
       EXTRACT(MONTH FROM SYSDATE) AS MONTHS,
       EXTRACT(DAY FROM SYSDATE) AS DAYS
  FROM DUAL; -- 提取年月日
SELECT EXTRACT(YEAR FROM SYSTIMESTAMP) AS YEARS,
       EXTRACT(MONTH FROM SYSTIMESTAMP) AS YEARS,
       EXTRACT(DAY FROM SYSTIMESTAMP) AS DAYS,
       EXTRACT(HOUR FROM SYSTIMESTAMP) AS HOURS,
       EXTRACT(MINUTE FROM SYSTIMESTAMP) AS MINUTES,
       EXTRACT(SECOND FROM SYSTIMESTAMP) AS SECONDS
  FROM DUAL; -- 提取年月日时分秒
```

### 查看注释

```sql
select 
      ut.COLUMN_NAME,--字段名称
      uc.comments,--字段注释
      ut.DATA_TYPE,--字典类型
      ut.DATA_LENGTH,--字典长度
      ut.NULLABLE--是否为空
from user_tab_columns  ut
inner JOIN user_col_comments uc
on ut.TABLE_NAME  = uc.table_name and ut.COLUMN_NAME = uc.column_name
where ut.Table_Name='WMIN10PO'
```

### to_char、to_date、to_number

```sql
SELECT SYSDATE,
       TO_CHAR(SYSDATE, 'YYYY-MM-DD'), -- 2018-09-02
       TO_CHAR(SYSDATE, 'MM'), -- 09
       TO_CHAR(SYSDATE, 'DD'), -- 02
       TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS'), -- 与下面的区别是，这是：2018-09-02 13:33:38，有0
       TO_CHAR(SYSDATE, 'FMYYYY-MM-DD HH24:MI:SS'), -- 与上面的区别是，这是：2018-9-2 13:33:38 没有0
       TO_CHAR(SYSDATE, 'YEAR-MONTH-DY') -- 打印出：TWENTY EIGHTEEN-9月 -星期日
  FROM DUAL;

SELECT TO_CHAR(123.456, '9999.9999') FROM DUAL; -- 输出 123.4560
SELECT TO_CHAR(123.456, '0000.0000') FROM DUAL; -- 输出 0123.4560
SELECT TO_DATE('1970-09-08', 'YYYY-MM-DD') FROM DUAL;-- 变为date类型（使用不多）

SELECT SYSDATE,
       TO_CHAR(SYSDATE, 'YYYY-MM-DD'), -- 2018-09-02
       TO_CHAR(SYSDATE, 'MM'), -- 09
       TO_CHAR(SYSDATE, 'DD'), -- 02
       TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS'), -- 与下面的区别是，这是：2018-09-02 13:33:38，有0
       TO_CHAR(SYSDATE, 'FMYYYY-MM-DD HH24:MI:SS'), -- 与上面的区别是，这是：2018-9-2 13:33:38 没有0
       TO_CHAR(SYSDATE, 'YEAR-MONTH-DY') -- 打印出：TWENTY EIGHTEEN-9月 -星期日
  FROM DUAL;

SELECT TO_CHAR(123.456, '9999.9999') FROM DUAL; -- 输出 123.4560
SELECT TO_CHAR(123.456, '0000.0000') FROM DUAL; -- 输出 0123.4560
SELECT TO_DATE('1970-09-08', 'YYYY-MM-DD') FROM DUAL;-- 变为date类型（使用不多）
```

### 判断

注意：数据类型必须一致，否则报错，工作当中就遇到了这个问题

```sql
SELECT NVL(val1,val2) FROM DUAL;  -- 如果val1为null的话，则返回val2，否则返回val1；
SELECT NVL2(val1,val2,val3) FROM DUAL; -- 如果val1为null的话，返回val3，否则返回val2；
SELECT NULLIF(val1,val2) FROM DUAL; -- val1和val2是否相等，如果相等则返回null，否则返回val1（注：如果val1为null，则报错）
-- decode
SELECT DECODE(val,val1,reslult1,val2,result2,...,default) FROM DUAL; -- 如果val等于val1，则返回result1；如果val等于val2，则返回result2.。。。。，否则返回default；
SELECT DECODE(T1.WON_ID, '1', '11',(SELECT T.CARD_NO FROM PMP_AGNT_WON_MARK_RESULT T WHERE T.CARD_NO = '450122198702054542')) 
  FROM PMP_AGNT_WON_MARK_RESULT T1 WHERE t1.card_no = '450122198702054542'; -- decode 里面的数据一定要唯一，里面的查询一定要括号
-- oracle 也可以case when
SELECT COALESCE(val1,val2,val3,...,valn) FROM DUAL; -- 如果val1为null，则返回val2，如果val2为null，则返回val3，如果valn还是null，则返回null；
```

### 交集、并集查询

```sql
SELECT1 UNION ALL SELECT2; -- 返回全部结果，包括重复
SELECT1 UNION SELECT2; -- 返回全部结果，去掉重复
SELECT1 MINUS SELECT2; -- 求出交集后，返回SELECT1去掉交集的数据
SELECT1 Intersect SELECT2; -- 返回交集
```

### 统计函数 

count,sum,avg,max,min,median,variance,stddev

```sql
SELECT COUNT(*) FROM emp; -- 求出所有列，返回17
SELECT COUNT(comm) FROM emp; -- emp表上的空值不被统计在内
SELECT COUNT(DISTINCT deptno) FROM emp; -- 消除重复值
```

### 行转列，列转行

```sql
SELECT t.county_name,LISTAGG(t.street_name,',') WITHIN GROUP (ORDER BY t.cover_id) FROM pmp_agnt_cover_info t WHERE t.city_name = '深圳市' GROUP BY t.county_name; 
SELECT id,to_char(wm_concat(name099)) FROM test099 GROUP BY ID; -- 1 a,c,b   2 d,e
```

### 备注

```sql
SELECT * FROM pmp_agnt_sign_info s WHERE REGEXP_LIKE(s.mark_code,'797Y00');

CREATE TABLE myemp AS SELECT * FROM emp;  -- 这个东西经常用，记住记住
CREATE TABLE myemp AS SELECT * FROM emp where 1 = 2;  -- 复制表结构，不要数据

-- 查看oracle版本
SELECT * FROM v$version;

-- 授权 Grant/Revoke object privileges 记住一个为表名，一个为登录账号哦
grant select, insert, update, delete, references, alter, index on PMP_AGNT_SUBJECT_QUOTATION to opadmin;
```

### 定义临时表

```sql
-- WITH e AS (sql语句)：对临时表进行使用哦，哈哈，好似oracle真的好叼；注意只为一个sql
WITH e AS (SELECT * FROM emp WHERE empno IN (7369,7499)) SELECT * FROM e WHERE e.empno = 7499;
```

### 更新插入

#### 超级大发现

```sql
UPDATE myemp SET (sal,comm) = (SELECT sal,comm FROM myemp WHERE empno = '7890') WHERE empno = '7369';
```

#### MERGE INTO 

参考自：https://blog.csdn.net/jeryjeryjery/article/details/70047022

```sql
-- 数据结构
MERGE INTO [target-table] A USING [source-table sql] B ON([conditional expression] and [...]...)
WHEN MATCHED THEN
  [UPDATE sql]
WHEN NOT MATCHED THEN
  [INSERT sql]

-- 数据准备
create table A_MERGE
(
  id   NUMBER not null,
  name VARCHAR2(12) not null,
  year NUMBER
);
create table B_MERGE
(
  id   NUMBER not null,
  aid  NUMBER not null,
  name VARCHAR2(12) not null,
  year NUMBER,
  city VARCHAR2(12)
);

insert into A_MERGE values(1,'liuwei',20);
insert into A_MERGE values(2,'zhangbin',21);
insert into A_MERGE values(3,'fuguo',20);
commit;
 
insert into B_MERGE values(1,2,'zhangbin',30,'吉林');
insert into B_MERGE values(2,4,'yihe',33,'黑龙江');
insert into B_MERGE values(3,3,'fuguo',NULL,'山东');
commit;

SQL> SELECT * FROM A_MERGE;
        ID NAME               YEAR
         1 liuwei               20
         2 zhangbin             21
         3 fuguo                20

SQL> SELECT * FROM B_MERGE;
        ID        AID NAME               YEAR CITY
         1          2 zhangbin             30 吉林
         2          4 yihe                 33 黑龙江
         3          3 fuguo                   山东

-- 哈哈
MERGE INTO A_MERGE A USING (select B.AID,B.NAME,B.YEAR from B_MERGE B) C ON (A.id=C.AID)
WHEN MATCHED THEN
  UPDATE SET A.YEAR=C.YEAR 
WHEN NOT MATCHED THEN
  INSERT(A.ID,A.NAME,A.YEAR) VALUES(C.AID,C.NAME,C.YEAR);
  COMMIT;

-- 变成结果 
 SQL> SELECT * FROM A_MERGE;
        ID NAME               YEAR
         1 liuwei               20
         2 zhangbin             30
         3 fuguo        
         4 yihe                 33
  
-- 也可以加where，这样的话就条件更新或者新增的了
merge into A_MERGE A USING (select B.AID,B.name,B.year,B.city from B_MERGE B) C 
ON(A.id=C.AID) 
when matched then
  update SET A.name=C.name where C.city != '江西'
when not matched then
  insert(A.ID,A.name,A.year) values(c.AID,C.name,C.year) where C.city='江西';
commit;
```

### 层级结构，树形结构处理  

START WITH ...... CONNECT BY ......

```sql
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00001', '中国', '-1');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00011', '陕西', '00001');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00012', '贵州', '00001');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00013', '河南', '00001');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00014', '广东', '00001');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00111', '西安', '00011');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00112', '咸阳', '00011');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00113', '延安', '00011');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00211', '贵阳', '00012');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00311', '洛阳', '00013');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00312', '郑州', '00013');
INSERT INTO DEMO (ID, DSC, PID) VALUES ('00411', '肇庆', '00014');

-- 从上到下，留意 ID = PID;
SELECT * FROM DEMO START WITH ID = '00001' CONNECT BY PRIOR ID = PID;
-- 从下到上，留意 PID = ID;
SELECT * FROM DEMO START WITH ID = '00112' CONNECT BY PRIOR PID = ID;
-- 可以用in
SELECT * FROM DEMO START WITH ID IN ('00011','00012','00013') CONNECT BY PRIOR ID = PID; 
-- 条件筛选，WHERE一定要是 START WITH 前面
SELECT * FROM DEMO WHERE ID != '00311' START WITH ID IN ('00011','00012','00013') CONNECT BY PRIOR ID = PID; 
```

### 分组

OVER(PARTITION BY......)   

```sql
-- 按照部门进行分区，over 操作分区后的数据(这里操作的是每个分区的工资总和)
SELECT t.deptno,t.ename,t.sal,SUM(t.sal) OVER (PARTITION BY t.deptno) AS cnt FROM emp t; 
-- 按照部门进行分区，over 操作分区后的数据（这里操作的是每个分区的最大工资）
SELECT t.deptno,t.ename,t.sal,MAX(t.sal)OVER(PARTITION BY t.deptno) AS cnt FROM emp t; 
SELECT t.deptno,t.ename,t.sal,MAX(t.sal)OVER() AS cnt FROM emp t; -- 没有进行分区，总共就只有一个分区，所以求的就是总的最大值
SELECT t.deptno,t.ename,t.sal,SUM(t.sal)OVER(PARTITION BY t.deptno,t.job) AS cnt FROM emp t;-- 对多个字段进行分区
-- 在分区内对工资进行降序（当然，也可有多个字段），求和递增，请测试
SELECT t.deptno,t.ename,t.sal,SUM(t.sal)OVER(PARTITION BY t.deptno ORDER BY t.sal DESC) AS cnt FROM emp t; 
SELECT t.deptno,t.ename,t.sal,SUM(t.sal)OVER(ORDER BY t.sal DESC) AS cnt FROM emp t; -- 所有数字求和递增
-- 根据身份证号码分区，然后取时间最大那条
SELECT tt.card_no1,tt.epiemp_name1 FROM (
  SELECT ROW_NUMBER() OVER (PARTITION BY t.card_no ORDER BY t.createtime DESC) AS rn,t.card_no AS card_no1,t.epiemp_name AS epiemp_name1 FROM pmp_agnt_exam_score t
)tt WHERE tt.rn = 1;
```

### 返回多行的子查询

咔堡oracle与mysql都可以的啊叼，一开始以为mysql不可以

```sql
-- 查看与 SCOTT 同一工作而且同一工资的人
SELECT * FROM emp t WHERE (job,sal) = (SELECT t1.job,t1.sal FROM emp t1 WHERE t1.ename = 'SCOTT') AND t.ename != 'SCOTT';
```

### 关于关键词ANY

```sql
-- 输出 2850,2975,2450
SELECT MIN(sal) FROM emp WHERE job = 'MANAGER' GROUP BY deptno; 

-- 效果与IN一样
SELECT * FROM emp t WHERE t.sal = ANY (SELECT MIN(sal) FROM emp WHERE job = 'MANAGER' GROUP BY deptno);

-- 查询所有高于以及等于子查询的最大值（这里具体是指2975）的所有数据
SELECT * FROM emp t WHERE t.sal > ANY (SELECT MIN(sal) FROM emp WHERE job = 'MANAGER' GROUP BY deptno);

-- 查询所有低于以及等于子查询的最小值（这里具体是指2450）的所有数据
SELECT * FROM emp t WHERE t.sal < ANY (SELECT MIN(sal) FROM emp WHERE job = 'MANAGER' GROUP BY deptno);
```

### EXIT

```sql
SELECT * FROM emp  -- 不返回任何数据嘛
WHERE EXISTS (SELECT * FROM emp WHERE empno = 99999); -- EXISTS 返回false，所以不会有任何数据返回 (类似于SELECT * FROM emp WHERE 1 = 2；1 = 2 也是返回false嘛)

SELECT * FROM emp -- 返回所有数据嘛
WHERE EXISTS (SELECT * FROM emp); -- EXISTS 返回true，所以不会有任何数据返回 (类似于SELECT * FROM emp WHERE 1 = 1；1 = 1 也是返回true嘛)

SELECT * FROM emp -- 返回所有数据嘛
WHERE EXISTS (SELECT * FROM emp WHERE empno = 7369); -- 一样是返回所有数据，因为子查询也是返回true嘛，不要被条件所迷惑

SELECT * FROM emp  -- 类似地，加个not，返回所有数据
WHERE NOT EXISTS (SELECT * FROM emp WHERE empno = 99999); 

-- 实际上是 pmp_agnt_mark_info 与 test_mark 的关联查询，厉害
UPDATE pmp_agnt_mark_info mi SET mi.mark_status = '发布' WHERE EXISTS (SELECT 1 FROM test_mark t WHERE mi.mark_code = t.mark_code AND mi.mark_batch = t.mark_batch AND t.mark_status = '发布');
```

### 事务

```sql
SET AUTOCOMMIT = OFF; -- 取消自动提交处理，开启事务处理
SET AUTOCOMMIT = ON;  -- 打开自动提交处理，关闭事务处理
COMMIT;  -- 提交事务
ROLLBACK TO  -- 回滚操作
SAVEPOINT;   -- 设置事务保存点

SQL> insert into myemp (empno,ename) values (1111,'aaaa');
SQL> update myemp set ename = 'bbbb' where empno = '1111';
SQL> savepoint sp_a;
SQL> update myemp set ename = 'cccc' where empno = '1111';
SQL> savepoint sp_b;
SQL> update myemp set ename = 'dddd' where empno = '1111';
SQL> savepoint sp_c;
SQL> select empno,ename from myemp where empno = '1111';

     EMPNO ENAME
---------- --------------------
      1111 dddd

已选择1行。

SQL> rollback to sp_b;
SQL> select empno,ename from myemp; -- 可以看到值与回滚点的关系，哈哈

     EMPNO ENAME
---------- --------------------
      1111 cccc

已选择1行。

SQL>
```

### declare, begin, end 代码块

```xml
<select id="reject" parameterType="java.util.Map" resultType="java.lang.Long">
 BEGIN
   UPDATE TSE_SAPFI_SUPPLIER SET STATUS = #{status,jdbcType=VARCHAR},MAIN_STATUS=#{mainStatus,jdbcType=VARCHAR} where user_id=#{userId} and MAIN_STATUS != '05'
   UPDATE SUPP_AUTHENTICATE SET AUTH_BY = #{authBy,jdbcType=VARCHAR},review_result=#{reviewResult,jdbcType=VARCHAR} where guid=#{auguid};
 END;
</select> 
      
<select id="review" parameterType="java.util.Map" resultType="java.lang.Long">
 DECLARE
   v_num number;
 BEGIN
   UPDATE TSE_SAPFI_SUPPLIER SET STATUS = #{status,jdbcType=VARCHAR} WHERE user_id=#{userId} and MAIN_STATUS != '06';
   UPDATE SUPP_AUTHENTICATE SET AUTH_BY = #{authBy,jdbcType=VARCHAR},review_result=#{reviewResult,jdbcType=VARCHAR} WHERE guid=#{auguid};    
   SELECT count(1) INTO v_num FROM pmp_agnt_auto_authenticate pa WHERE pa.auth_id = #{auguid};
   IF v_num > 0 THEN
     UPDATE tse_sapfi_supplier ts SET ts.main_status='06' WHERE user_id=#{userId};
     UPDATE supp_authenticate  sa SET sa.main_status='06' WHERE guid=#{auguid}; 
   END IF;  
 END;
</select> 

<select id="getAgntCfgByAgr" parameterType="com.sf.pmp.tse.model.agreementNew.AgreementNew" resultType="java.util.Map">
   <if test="bussinessType=='JT'">
       select examine_indexs,calcu_formulas from pmp_agnt_examine_cfg ec
            where  
       ec.area_code = #{bussinessArea} and ec.item_type='2' and #{agrStartDate}  between ec.begin_date and ec.end_date
   </if>
   <if test="bussinessType=='ZZFW'">
       select examine_indexs||'收件计提比例'||calcu_formulas||'收件固定计提'||WEIGHTS||'派件计提比例'||CHECK_NO||'派件固定计提'||EXT2||'计算规则'||EXT1 AS examine_indexs from pmp_agnt_examine_cfg ec
            where  
       ec.area_code = #{bussinessArea} and ec.item_type='3' and #{agrStartDate}  between ec.begin_date and ec.end_date
   </if>
</select>
```

```sql
begin
 FOR i IN 1..100000 LOOP
   insert into PMP_111_TEST (card_no) values (sys_guid());
   commit;
   end loop;
end;

begin
 FOR i IN 1..1000 LOOP
   insert into PMP_111_TEST(id,Card_No,Card_No_Enc)
   select i id ,bb.card_no,'' Card_No_Enc from PMP_AGNT_TEMP_DEAL_PASSWORD bb;
   commit;
   end loop;
end;


DECLARE
   v_old_resid NUMBER; -- 旧菜单id
   v_new_resid NUMBER; -- 新菜单id
   v_new_path VARCHAR(500); -- 新菜单路径
BEGIN
   SELECT t.resid INTO v_old_resid FROM pmp.sys_res t WHERE t.resname = '标的报价表(旧)';
   SELECT t.resid INTO v_new_resid FROM pmp.sys_res t WHERE t.resname = '标的报价表';
   SELECT PATH INTO v_new_path FROM pmp.sys_res t WHERE t.resname = '标的报价表';
   
   
   dbms_output.put_line(v_old_resid);
   dbms_output.put_line(v_new_resid);
   dbms_output.put_line(v_new_path);
      
   INSERT INTO pmp.sys_res
       (resid, resname, alias, sn, icon, parentid, defaulturl, isfolder, isdisplayinmenu, isopen, systemid, path, enabled, ressize)
       (
        SELECT resid+20000000000000,
               resname, 
               alias, 
               sn, 
               icon, 
               v_new_resid,
               defaulturl, isfolder, isdisplayinmenu, isopen, systemid, 
               CONCAT(CONCAT(v_new_path,':'),resid+20000000000000) AS PATH, 
               enabled, 
               ressize 
        FROM Sys_res t WHERE t.parentid = v_old_resid
       );
         
    FOR cur IN (
           SELECT resid,resname FROM pmp.sys_res t2 WHERE t2.parentid = v_old_resid
         )LOOP
            
            INSERT INTO pmp.sys_lan_res 
             (id, res_id, lan_type, lan_msg, remark)
             ( 
              SELECT t2.id+20000000000000,cur.resid+20000000000000,t2.lan_type,cur.resname,0 FROM sys_lan_res t2 WHERE t2.res_id = cur.resid
             ); 
    END LOOP;  
    
END;
```

### 分页

```sql
-- 取前面5条数据的最简单的方法（这个一定要记住）
SELECT * FROM pmp_agnt_won_mark_result WHERE ROWNUM <= 5;  

-- 取前面5条数据（通用方法）

   SELECT t.*,ROWNUM rn 
   FROM (SELECT * FROM pmp_agnt_won_mark_result)t
   WHERE ROWNUM <= 5;  -- 这里的 ROWNUM 是指 from 那里的 ROWNUM

-- 取第3到第5条数据（通用方法）
SELECT * FROM (
   SELECT t.*,ROWNUM rn   
   FROM (SELECT * FROM pmp_agnt_won_mark_result)t  -- 1）首先要将查询的数据 from(sql)一下
   WHERE ROWNUM <= 5  -- 2）前面5条
) t2 WHERE rn >= 3;  -- 取from的rownum  3）再取3到5条

SELECT t2.* FROM (
  SELECT t.*,ROWNUM rn FROM pmp_agnt_won_mark_result t  -- 看到了没有，rownum 又是取from后面的了
)t2 WHERE rn BETWEEN 3 AND 5;

SELECT * FROM (
    SELECT t.*,ROWNUM rn FROM pmp_agnt_won_mark_result t WHERE ROWNUM <= 5
) t2 WHERE t2.rn >= 3;     -- 又是from 后面的


-- 醍醐灌顶
SELECT t.*,Rownum FROM pmp_agnt_won_mark_result t WHERE rownum >= 3 AND ROWNUM <= 5;
-- 当生成结果集时，Oracle首先会产生一条ROWNUM为1的记录，显然不符合条件，那么同样会继续产生第二条数据，同样标识ROWNUM为1，
-- 该条记录同样继续被过滤掉，后续生成的ROWNUM依然为1，因此上述查询语句不会有任何查询结果，
-- 所以如果想要使上述结果有满足条件的结果集，必须使用子查询，代码如下：
SELECT * FROM 
(SELECT ROWNUM rn,t.* FROM pmp_agnt_won_mark_result t)t2 
WHERE rn >= 3 AND rn <= 5;
```

### 锁

排他锁（Exclusive,简称X锁）、共享锁（Share，简称S锁）

```sql
select empno,ename from myemp where deptno = 10 for update; -- 行级锁定，类似于INSERT,UPDATE,DELETE等DML语句
 
 SESSION 1:
 SQL> select empno,ename from myemp where deptno = 10 for update; -- 对相关数据实现了行级锁定
 
 SESSION 2;
 SQL>  select empno,ename from myemp where deptno = 10 for update; -- 处于一直等待状态
 
 SESSION 3;
 SQL>  select empno,ename from myemp where deptno = 10 for update nowait; -- 一发觉有其它锁锁定该数据，立刻抛出异常（nowait就是不要等待的意思嘛）
 select empno,ename from myemp where deptno = 10 for update nowait
*
第 1 行出现错误:
ORA-00054: 资源正忙, 但指定以 NOWAIT 方式获取资源

SESSION 4;
SQL>  select empno,ename from myemp where deptno = 10 for update wait 3;  -- 等待了3秒之后，抛出异常提示
 select empno,ename from myemp where deptno = 10 for update wait 3
                         *
第 1 行出现错误:
ORA-30006: 资源已被占用; 执行操作时出现 WAIT 超时

排他锁（Exclusive,简称X锁）：一旦用户对某个资源添加了X锁，则其他用户都不能再对该资源添加任何类型的锁，直到该用户释放了资源上的X锁为止。
共享锁（Share，简称S锁）：一旦用户对某个资源添加了S锁，则其他用户都不能在该资源上添加X锁，只能添加S说句，直到该用户释放了资源上的S锁为止。
```

### 其它数据库对象

```sql
-- 视图
CREATE VIEW v_myview AS sql;
SELECT * FROM v_myview;
DROP VIEW v_myview;
SELECT * FROM user_views; -- 查看所有已创建的视图

-- 序列
CREATE SEQUENCE myseq; -- 最简单的
SELECT * FROM user_sequences;  -- 查看所有的已创建的序列
SELECT myseq.nextval FROM dual; -- 求下一个序列
SELECT myseq.currval FROM dual; -- 求当前序列，注意：如果是刚刚创建序列的时候，立即执行这个语句会报错的
DROP SEQUENCE myseq;
-- ROWID：orcale 默认为每条记录分配一个唯一的地址编码
-- ROWNUM：目前所知是分页用

-- oracle 自动帮助用户创建，用户只需要使用即可
-- oracle 伪列
NEXTVAL,CURRVAL,SYSDATE,SYSTIMESTAMP,ROWNUM,ROWID
-- 伪表
dual

-- 索引
SELECT * FROM USER_INDEXES t WHERE t.TABLE_NAME = 'PMP_AGNT_WON_MARK_RESULT'; -- 查看所有表或者某一张具体表的索引，名称，类型等
SELECT t.index_name,t.table_name,t.column_name,t.column_position,t.column_length,t.char_length,t.descend FROM USER_IND_COLUMNS t WHERE t.INDEX_NAME = 'WON_ID';

SELECT * FROM User_Objects t WHERE t.OBJECT_NAME = 'PMP_AGNT_BASE_INFO'; -- 可以查看这个表什么时候创建，上一次DDL是什么时候等
SELECT * FROM user_tab_columns WHERE table_name= 'PMP_AGNT_BASE_INFO'; -- 对每个表的字段的详细信息进行查看（并不是指注释），曾经用这个东西来根据表名作为参数得到所有的字段参数

SELECT * FROM All_Tab_Comments t WHERE t.OWNER = 'PMP'; -- 查看所有表信息

select * from user_tab_columns where Table_Name='PMP_AGNT_BASE_INFO';
select * from all_tab_columns where Table_Name='PMP_AGNT_BASE_INFO';  -- 比  user_tab_columns 多了owner字段
select * from dba_tab_columns where Table_Name='PMP_AGNT_BASE_INFO';  -- 不知道 all_tab_columns 与 dba_tab_columns 有什么区别

SELECT * FROM USER_TABLES t;  -- 当前用户拥有的表
SELECT * FROM ALL_TABLES t;   -- 所有用户的表
SELECT * FROM DBA_TABLES t;   -- 系统表

SELECT distinct t.OWNER FROM DBA_TABLES t;
SELECT * FROM all_source;
SELECT DISTINCT t.TYPE FROM all_source t WHERE t.OWNER = 'PMP';
SELECT t.TYPE FROM all_source t WHERE t.OWNER = 'PMP' GROUP BY t.TYPE;-- 存储过程

-- 有时候非常有用
SELECT DISTINCT t.name,t.TYPE FROM all_source t WHERE t.OWNER = 'PMP' AND t.name = 'PMP_AGNT_PUBLIC_PKG';
SELECT * FROM all_source t WHERE t.OWNER = 'PMPSIT' AND t.name = 'PMP_AGNT_PUBLIC_PKG';
SELECT * FROM all_source t WHERE t.OWNER = 'PMPSIT' AND t.name = 'PMP_AGNT_PUBLIC_PKG' AND t.TEXT LIKE '% pmp_agnt_examine_cfg%'; 
SELECT * FROM all_source t WHERE t.OWNER = 'PMPSIT' AND t.TEXT LIKE '% pmp_agnt_examine_cfg%';
```

### clob

```sql
dbms_lob.substr(t.return_msg);  -- clob 转string
DBMS_LOB.GETLENGTH(t.return_msg)  -- 求clob长度

-- 如果clob的长度大于4000，可以这样处理
DECLARE
 SEND_MSGREALLY CLOB := '{"exeId":"1546395123116A38AB2A651E25D66416"}';
 RETURN_MSGREAL CLOB := '超级超级大的超过4000的字段';
BEGIN

  INSERT INTO PMP_PAY_INT_LOGA
  (SEND_ID,
   SEND_TM,
   RETURN_TM,
   SEND_ITEM,
   SEND_ERROR,
   SEND_MSG,
   RETURN_MSG,
   COL1,
   COL2,
   CARD_NO_ENC)
VALUES
  (10000779804973,
   TO_DATE('02-01-2019 10:12:08', 'dd-mm-yyyy hh24:mi:ss'),
   TO_DATE('02-01-2019 10:12:08', 'dd-mm-yyyy hh24:mi:ss'),
   'saveSignQueryLog',
   '',
   SEND_MSGREALLY,
   RETURN_MSGREAL,
   '',
   '350426199303111020',
   '');
end
```

