###  衣捞甲杂修改测试

```shell
注释：空格，再一个#或者两个井号##，再空格写注释
telnet ip port   ##  你准备好了吗，初次使用还要在windows上面勾上
ctrl + l   ## 清屏好使好多
lsb_release -a  ## 查看服务器信息
```

###  yum

+ wget：yum install wget
+ rz/sz：yum install lrzsz
+ gcc：yum install -y gcc-c++

###  diff;man;pwd;--help;echo

```shell
diff 123.txt 456.txt   # 差异
man  ## 查看帮助文档  (一般外部命令用这个)
pwd ## 查看当前所在的工作目录的全路径
ls --help ## 授之预渔(一般内部命令用这个) 

[root@mini1 ~]# a="是发放"
[root@mini1 ~]# echo a
a
[root@mini1 ~]# echo $a
是发放

## 打印当前进程号
[root@mini1 mypractice]# echo $$
29514
```

###  date;cal;日期校准

```shell
[root@mini1 ~]# date
Sat Aug 18 10:55:16 CST 2018
[root@mini1 ~]# date +%Y-%m-%d
2018-08-18
[root@mini1 ~]# date +%Y-%m-%d --date="-1 day"
2018-08-17
[root@mini1 ~]# date +%Y-%m-%d --date="-1 month"
2018-07-18
[root@mini1 ~]# date +%Y-%m-%d --date="1 year"
2019-08-18 

cal  ## 这个命令很有意思哦
cal 2018 ## 这个命令很有意思哦

yum install ntpdate
ntpdate ntp1.aliyun.com
```

###  last

```shell
[root@mini1 ~]# last -3
root     pts/0        192.168.174.1    Sat Aug 18 10:12   still logged in   
root     tty1                          Sat Aug 18 10:12   still logged in   
reboot   system boot  2.6.32-573.el6.x Sat Aug 18 10:11 - 11:00  (00:49)    

wtmp begins Sat Jul 14 21:42:45 2018
[root@mini1 ~]# last -4
root     pts/0        192.168.174.1    Sat Aug 18 10:12   still logged in   
root     tty1                          Sat Aug 18 10:12   still logged in   
reboot   system boot  2.6.32-573.el6.x Sat Aug 18 10:11 - 11:00  (00:49)    
root     pts/0        192.168.174.1    Sat Aug 18 00:51 - 01:02  (00:11)    

wtmp begins Sat Jul 14 21:42:45 2018
```

###  who;id;whoami 

```shell
[xiaofeng01@mini1 ~]$ id
uid=500(xiaofeng01) gid=500(xiaofeng01) groups=500(xiaofeng01) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[root@mini1 ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

[xiaofeng01@mini1 ~]$ whoami
xiaofeng01
[root@mini1 ~]# whoami
root
```

###  cd;ls

```shell
cd ~      ## 切换到用户主目录
cd  ## 什么路径都不带，则回到当前用户的主目录（比如当前用户是root，所以就回到/root，如果当前用户是xiaofeng01，所以就到/home/xiaofeng01目录去）
cd -  ## 回退到上次所在的目录

## ls 默认按照文件名排序
ls -lt    ## 时间降序排序
ls -lrt   ## 时间升序排序
ls -a     ## 显示出隐藏文件、
ls -lh    ## 人类可看的大小
ls -la
-l  长列表
-t  时间顺序，最新的在最前面
-r  -t的reverse

## 当前列表文件筛选
ls -lrt *pop3*
ls -lrt | grep *pop3*
```

###  mkdir;rm

```shell
mkdir aaa    ## 这是相对路径的写法
mkdir  /data   ## 这是绝对路径的写法
mkdir -p  aaa/bbb/ccc ##  级联创建目录，尼玛这个居然不知道自己有记笔记
rmdir  aaa  ## 可以删除空目录
rm  -r  aaa ## 级联删除
rm  -rf  aaa  ##  强制级联删除aaa
rm -rf aaa bbb ccc ## 哈哈哈，同时删除aaa,bbb,ccc这些文件夹啊
rm -rf *.xml *.txt  ## 删除所有后缀名为xml以及txt 的文件
```

###  touch;mv;cp;scp

 + touch

     + 创建文件haha：touch haha

+ cp

    + cp -rf aaa bbb：
    + 复制目录的时候，要加-r，表示递归recusive，
    + -f，覆盖已经存在的目标文件而不给出提示。

 + mv

     + 将文件haha改名为xixi：mv haha xixi
     + 将文件xixi移动到当前目录下面的文件夹heihei内：mv xixi heihei

 + scp

    + -r：递归的作用
    + 可以从本服务传输到其它服务器，也可以从其它服务器传输到本服务器
    + system copy，服务间文件拷贝，要输入用户名密码的登录：scp 【参数】【原路径以及文件】【目标路径】

   ```shell
   scp -r /etc/hosts root@mini3:/etc/
   ```

###  vi

```shell
1)首先进入 “一般模式”：
gg   直接跳到文件的首行
G    直接跳到文件的末行
dd    删除一行
yy    复制一行
p     粘贴
u     撤销
$     移动到这一行的行尾
2) 按 i 键，就会从一般模式进入“编辑模式”
3) “编辑模式” 按 esc 再按：（冒号）进入 “底行模式”：
n #n为数字。光标移动到第n 行
/ #寻找内容
%s/word1/word2/g #从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2
n1,n2s/word1/word2/g #n1 与 n2 为数字。在第 n1 与 n2 行之间寻找 word1 这个字符串，并将该字符串取代
为 word2
set nu #显示行号
set nonu #取消行号
q! #强制离开不保存
wq #离开并保存
wq! #强制离开并保存
!ls #暂时离开
```

###  wc 统计行

```shell
[root@SYSOPS00074145 gxf]# wc -l 123.txt ## 说明123.txt这个文件里面有5行
5 123.txt  
```

###  grep

```shell
grep 'fffff' 123.txt   ## 筛选
grep -n 'fffff' 123.txt ## -n 显示行数
grep -w 'fffff' 123.txt ## -w 精确匹配
grep -i 'fffff' 123.txt ## -i 忽略大小写
grep -v 'fffff' 123.txt ## -v 反向选择
grep 'fffff' 123.txt | wc -l ## 管道
grep -i 'fffff' 123.txt | wc -l ##管道
```


###  cat;more;less;tail;head 

```shell
## 一次性将文件内容全部输出（控制台）
cat  
## more命令可以选择性忽略了
## -m  显示百分比，我觉得这个很有用
## q 退出
## PageDown  PageUp 上下翻页，其它相关命令都可以不用记了
## home键和end键分别是第一行和最后一行，其它相关命令都可以不用记了
## u 向上翻半页（up）  d 向下翻半页（down）
## 方向键向上下可以移动一行，其它相关命令可以不用记了
## 搜索关键字（/keyword） N/n 上/下一个关键字（next）
less

## 如果不加 -10 的话，就默认显示前/后10行
tail -10  install.log  查看文件尾部的10行
tail -n 10  install.log  查看文件尾部的10行
head  -10  install.log   查看文件头部的10行
tail -f install.log  实时跟踪文件
tail -10f install.log  实时跟踪文件，开始时显示后10行	
## 显示这个文件的第八行
[root@mini1 mypractice]# head -8 right.out | tail -1
ccc.txt
```

### cut

+ -d #指定分割符

+ -f #指定截取区域

+ -c #以字符为单位进行分割

  注意：不加-d选项，默认为制表符，不是空格 

```shell
## 以':'为分隔符，截取出/etc/passwd的第一列跟第三列 
cut -d ':' -f 1,3 /etc/passwd
## 以':'为分隔符，截取出/etc/passwd的第一列到第三列 
cut -d ':' -f 1-3 /etc/passwd
## 以':'为分隔符，截取出/etc/passwd的第二列到最后一列 
cut -d ':' -f 2- /etc/passwd
## 截取/etc/passwd文件从第二个字符到第九个字符 
cut -c 2-9 /etc/passwd
## 比如领导想叫你截取linux上面所有可登陆普通用户,-v 表示取反，不加-v的话，就表示过滤出root用户了
cat /etc/passwd | grep '/bin/bash' | cut -d ':' -f 1 | grep -v root
```

### awk

***awk '条件1 {执行动作} 条件2 {执行动作} ...' 文件名***

***awk [选项] '条件1 {执行动作} 条件2 {执行动作} ...' 文件名***

+ printf #格式化输出，不会自动换行

+ print #打印出内容，默认会自动换行

+ %s #代表字符串

+ \t #制表符

+ \n #换行符

+ NR #行号

+ $1 #代表第一列

+ $2 #代表第二列

+ $NF#代表最后一列

+ -F #指定分割符

  ```shell
  [root@mini1 aaa]# printf '%s\t%s\t\n' 1 2
  1       2
  ```

  > 打印出第二列：df -h | awk '{print $2}'
  >
  > 打印出最后一列：df -h | awk '{print $NF}'
  >
  > 打印出第四行的第二列：df -h | awk 'NR==4 {print $2}'
  >
  > 打印出第二到第五行的第一列：df -h | awk '(NR>=2 && NR <=5) {print $1}' 
  >
  > 以:为文件分隔符，打印出第一行：cat /etc/passwd | awk '{FS=":"}{print $1}'、awk -F":" '{print $1}' /etc/passwd 

  ```shell
  [root@mini1 aaa]# df -h
  Filesystem            Size  Used Avail Use% Mounted on
  /dev/mapper/vg_mini1-lv_root
                         18G  7.9G  8.5G  49% /
  tmpfs                 1.5G   80K  1.5G   1% /dev/shm
  /dev/sda1             477M   37M  415M   9% /boot
  /dev/sr0              3.7G  3.7G     0 100% /media/CentOS_6.7_Final
  [root@mini1 aaa]# df -h | awk '{printf $1} {printf "文件系统使用率："} {print $5}'                      
  Filesystem文件系统使用率：Use%
  /dev/mapper/vg_mini1-lv_root文件系统使用率：
  18G文件系统使用率：/
  tmpfs文件系统使用率：1%
  /dev/sda1文件系统使用率：9%
  /dev/sr0文件系统使用率：100%
  [root@mini1 aaa]# df -h |grep -v 'Filesystem' | awk '{printf $1} {printf "文件系统使用率："} {print $5}'
  /dev/mapper/vg_mini1-lv_root文件系统使用率：
  18G文件系统使用率：/
  tmpfs文件系统使用率：1%
  /dev/sda1文件系统使用率：9%
  /dev/sr0文件系统使用率：100%
  [root@mini1 aaa]# df -h |grep -v 'Filesystem' | awk 'BEGIN {printf "文件系统使用情况：\n \n"} {printf $1} {printf "文 件 系统使用率："} {print $5}'
  文件系统使用情况：
   
  /dev/mapper/vg_mini1-lv_root文 件系统使用率：
  18G文 件系统使用率：/
  tmpfs文 件系统使用率：1%
  /dev/sda1文 件系统使用率：9%
  /dev/sr0文 件系统使用率：100%
  [root@mini1 aaa]# df -h |grep -v 'Filesystem' | awk 'BEGIN {printf "文件系统使用情况：\n \n"} {printf $1} {printf "文件系统使用率："} {print $5} END {printf "一切正常 \n"}'
  文件系统使用情况：
   
  /dev/mapper/vg_mini1-lv_root文件系统使用率：
  18G文件系统使用率：/
  tmpfs文件系统使用率：1%
  /dev/sda1文件系统使用率：9%
  /dev/sr0文件系统使用率：100%
  一切正常 
  ```


### sed

+ 主要对数据进行处理（选取，新增，替换，删除，搜索）
+ sed [选项] [动作] 文件名
+ -n #把匹配到的行输出打印到屏幕
+ p #以行为单位进行打印，通常与-n一起使用

1. 打印出第二行：df -h | sed -n '2p'
2. 删除第二行：df -h | sed '2d' 
3. a #在行的下面插入新的内容：df -h | sed '2a 1234567890' 
4. i #在行的上面插入新的内容：df -h | sed '2i 1234567890'
5. c #替换：df -h | sed '2c 1234567890' 
6. 指定字符串替换：s/要被取代的内容/新的字符串/g #指定内容进行替换：df -h | sed 's/centos-root/Centos7/g'
7. 在文件中模糊搜索tmpfs：sed -n '/tmpfs/p' df.txt （ grep  tmpfs 123.txt ）
8. -e #表示可以执行多条动作：sed -e 's/18/188/g' -e 's/477/488/g' 123.txt > 444.txt（替换18为188，替换477位488），并输出所在行

### crond

```shell
*  *  * *  * 级别 命令 
分 时 日 月 周

service crond status/start/stop/restart
crontab -l #列出crontab有哪些任务 
crontab -e #编辑crontab任务 
crontab -r #删除crontab里的所有任务

* * * * * /home/hahahome/timer/test.sh >> /home/hahahome/timer/haha.txt
* * * * * /home/hahahome/timer/test02.sh >> /home/hahahome/timer/haha.txt
```

+ 每分钟执行：\* * * * * 或者 */1 * * * *
+ 每小时执行 ：0 * * * *
+ 每天执行：0 0 * * * 
+ 每周执行：0 0 * * 0 
+ 每月执行 ：0 0 1 * *
+ 每年执行 ：0 0 1 1 *
+ 每天早上6点执行 ：0 6 * * *
+ 每两个小时执行 ：0 */2 * * *
+ 每小时的10分，40分执行 ：10,40 * * * *
+ 每天的下午4点、5点、6点的5 min、15 min、25 min、35 min、45 min、55 min时执行命令：5,15,25,35,45,55 16,17,18 * * *

###  重定向

```shell
posix名称 文件描述符 用途
/dev/stdin 0 标准输入（默认，不填写就代表这个）
/dev/stdout 1 标准输出
/dev/stderr 2 标准错误输出

## 进入编辑页面，然后你所输入的文字将覆盖这个文件
	cat > 123.txt 
## 进入编辑页面，然后你所输入的文字将追加这个文件的后面
	cat >> 123.txt
## 输出重定向：将 ls -lrt 的内容追加到 123.txt 这个文件上面去
	ls -lrt >> 123.txt 
## 输出重定向：将 echo "asdfasfdafaf" 这个内容覆盖到文件 456.txt 里面
	echo "asdfasfdafaf"  > 456.txt
## 错误输出重定向：llll 这个命令错误，将这个错误命令信息打印覆盖到 123.txt 这个文件
	llll 2> 123.txt
## 错误输出重定向：llll 这个命令错误，将这个错误命令信息打印追加到 123.txt 这个文件
	llll 2>> 123.txt
## ;代表的是可以执行多条命令；例如这个例子，把这个东东合并输出
	[root@mini1 mypractice]# cat a1.txt ;cat a2.txt 
## &&：前面的命令执行成功的话，后面的才可以执行成功；前面的命令执行失败的话，后面的不可以执行
## ||：前面的命令执行成功的话，后面的不可以执行；前面的命令执行失败的话，后面的可以执行
	[root@mini1 mypractice]# lll a1.txt && cat a2.txt   
	-bash: lll: command not found
	[root@mini1 mypractice]# lll a1.txt || cat a2.txt   
	-bash: lll: command not found
	2222
## &>：代表不分正确还是错误的意思
## 也可以这样写：lll 11151561 > fff.txt 2>&1
## 也可以这样写：lll 11151561 >> fff.txt 2>&1
	[root@mini1 mypractice]# cat fff.txt 
	-bash: afasfs: command not found
	-bash: nmiomo: command not found
	[root@mini1 mypractice]# lll sdfafdasf &> fff.txt 
	[root@mini1 mypractice]# cat fff.txt              
	-bash: lll: command not found
	[root@mini1 mypractice]# lll 11151561 &>> fff.txt          
	[root@mini1 mypractice]# cat fff.txt              
	-bash: lll: command not found
	-bash: lll: command not found
	[root@mini1 mypractice]# lljijl 11151561 &>> fff.txt 
	[root@mini1 mypractice]# cat fff.txt                 
	-bash: lll: command not found
	-bash: lll: command not found
	-bash: lljijl: command not found
## 把错误的输出到 wrong.out，把正确的输出到 right.out，注意：1>  2>
	ls ./ afasf.txt 1> right.out 2> wrong.out	
## set -C 禁止覆盖；>| 强制覆盖；set +C 解除禁止覆盖
	[root@mini1 mypractice]# set -C
	[root@mini1 mypractice]# ll > a1.txt 
	-bash: a1.txt: cannot overwrite existing file
	[root@mini1 mypractice]# ll >| a1.txt 
	[root@mini1 mypractice]# cat a1.txt 
	total 68
	-rw-r--r--. 1 root root     0 May 17 01:08 a1.txt
	-rw-r--r--. 1 root root     5 May 16 23:46 a2.txt
	-rw-r--r--. 1 root root    10 May 16 23:47 a3.txt
	-rw-r--r--. 2 root root  5502 May 15 06:19 aaa.txt
	-rw-r--r--. 2 root root  5502 May 15 06:19 AAA.txt
	lrwxrwxrwx. 1 root root     7 May 16 18:55 BBB.txt -> bbb.txt
	-rw-r--r--. 1 root root   279 May 16 23:13 ccc.txt
	-rw-r--r--. 1 root root    42 May 16 23:35 ddd.txt
	-rw-r--r--. 1 root root   670 May 16 23:41 eee.txt
	-rw-r--r--. 1 root root    93 May 17 00:54 fff.txt
	-rw-r--r--. 1 root root     0 May 17 00:26 ggg.txt
	-rw-r--r--. 1 root root     0 May 17 01:04 list.err
	-rw-r--r--. 1 root root   737 May 17 01:04 list.txt
## /dev/null ，可以说成是黑洞装置。为空，即不保存。
	[root@mini1 mypractice]# ls 2> /dev/null
	a1.txt  a3.txt   AAA.txt  ccc.txt  eee.txt  ggg.txt   list.txt
	a2.txt  aaa.txt  BBB.txt  ddd.txt  fff.txt  list.err  logTest.txt
	[root@mini1 mypractice]# llll 2> /dev/null
## read命令 从屏幕读取输入流 agsagdsadg，读完之后再输出流 输出到变量 a；一旦换行就立刻终止
	[root@mini1 fd]# read a
	agsagdsadg
	[root@mini1 fd]# echo $a
	agsagdsadg
## 这个时候，变量a的输入不是从屏幕的了，而是从文件 a1.txt 的了，但是只能获取文件的第一行哦
	[root@mini1 mypractice]# cat a1.txt 
	cat: asdf.txt: No such file or directory
	csdfadfafdat: asdf.txt: No such file or directory
	[root@mini1 mypractice]# read a 0< a1.txt 
	[root@mini1 mypractice]# echo $a
	cat: asdf.txt: No such file or directory
```

###  压缩/解压 

1）tar为打包；压缩的话只能压缩文件，不能压缩文件夹；只测试过前面两三个，后面的没有测试过，只是用来备份记录

2）要区分打包到当前文件夹还是指定文件夹的命令的不同之处

```shell
.tar.gz 
解压：tar zxvf FileName.tar.gz  
解压到指定文件夹：tar -zxvf jdk-8u181-linux-x64.tar.gz -C /usr/local  ## 注意 -C ，redis练习就是用这个
压缩：tar zcvf FileName.tar.gz DirName 
--------------------------------------------- 
.zip
解压：unzip FileName.zip ## 顺丰就是用这个
压缩：zip FileName.zip DirName 
--------------------------------------------- 
.tar
压缩：tar cvf FileName.tar FileName
解压：tar xvf FileName.tar
--------------------------------------------- 
.gz
解压1：gunzip FileName.gz 
解压2：gzip -d FileName.gz 
压缩：gzip FileName 
--------------------------------------------- 
.bz2 
解压1：bzip2 -d FileName.bz2 
解压2：bunzip2 FileName.bz2 
压缩： bzip2 -z FileName 
.tar.bz2 
解压：tar jxvf FileName.tar.bz2 
压缩：tar jcvf FileName.tar.bz2 DirName 
--------------------------------------------- 
.bz 
解压1：bzip2 -d FileName.bz 
解压2：bunzip2 FileName.bz 
压缩：未知 
.tar.bz 
解压：tar jxvf FileName.tar.bz 
压缩：未知 
--------------------------------------------- 
.Z 
解压：uncompress FileName.Z 
压缩：compress FileName 
.tar.Z 
解压：tar Zxvf FileName.tar.Z 
压缩：tar Zcvf FileName.tar.Z DirName 
--------------------------------------------- 
.tgz 
解压：tar zxvf FileName.tgz 
压缩：未知 
.tar.tgz 
解压：tar zxvf FileName.tar.tgz 
压缩：tar zcvf FileName.tar.tgz FileName 
--------------------------------------------- 
.rar 
解压：rar a FileName.rar 
压缩：rar e FileName.rar
```

### find

-name 指查找文件名；-type 指要查找的文件类型；-perm  文件权限；-user 所属主

```shell
find -name sfapp   当前目录下查询是否有sfapp这个文件或者文件夹
find / -name sfapp  根目录下查询
find /home -name sfapp 指定目录下查找
find /home -name sfa*  模糊查找
find /home -name *fa* 模糊查找
find /home -type d -name 111  ### d表示目录  
find /home -type f -name 111  ### f表示文件
查找可执行的命令所在的路径：which java
查找可执行的命令和帮助的位置：whereis java
显示历史命令：history
histroy | grep srp   查看关于输过srp的历史命令
```

### 软硬链接

+ 为文件111（注意一定要是写绝对路径），在 ../bbb 这个路径下面创建一个软链接 222，就类似于桌面快捷方式，一旦源文件：/home/hahahome/aaa/111删掉，就会闪烁

```sh
ln -s  /home/hahahome/aaa/111 ../bbb/222  ## /home/hahahome/aaa
222 -> /home/hahahome/aaa/111 ## ../bbb
```

 + 为文件111（注意一定要是写绝对路径），在 ../bbb 这个路径下面创建一个软链接 222，111 和 222 都是一个变量，他们的地位相等，都指向底层的一个对象，删除其中谁谁谁都没有关系。他们的Inode号一样。常用于防止重要文件被误删

   ```shell
   [root@mini1 aaa]# ln  /home/hahahome/aaa/111 ../bbb/222
   [root@mini1 aaa]# stat 111
     File: `111'
     Size: 0               Blocks: 0          IO Block: 4096   regular empty file
   Device: fd00h/64768d    Inode: 282021      Links: 2
   Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
   Access: 2020-11-19 02:16:20.108344869 +0800
   Modify: 2020-11-19 02:16:20.108344869 +0800
   Change: 2020-11-19 02:16:25.016344645 +0800
   [root@mini1 aaa]# cd ../bbb
   [root@mini1 bbb]# ll
   total 0
   -rw-r--r--. 2 root root 0 Nov 19 02:16 222
   [root@mini1 bbb]# stat 222
     File: `222'
     Size: 0               Blocks: 0          IO Block: 4096   regular empty file
   Device: fd00h/64768d    Inode: 282021      Links: 2
   Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
   Access: 2020-11-19 02:16:20.108344869 +0800
   Modify: 2020-11-19 02:16:20.108344869 +0800
   Change: 2020-11-19 02:16:25.016344645 +0800
   [root@mini1 bbb]# ls -li
total 0
   282021 -rw-r--r--. 2 root root 0 Nov 19 02:16 222
   ```
   
### 权限

####  文件权限

| d    | rwxr-xr-x | 1                  | root | root | 1595 | Nov 10  2018 | anaconda-ks.cfg |
| ---- | --------- | ------------------ | ---- | ---- | ---- | ------------ | --------------- |
|      |           | 上面有多少个硬链接 |      |      | 大小 | 时间         | 文件名          |

   \- #代表的是文件；d#代表是目录； l #软链接文件 ；b #代表块设备；c #代表的是硬件设备（键盘） 
   r:  对文件来说，是可读取内容；  对文件夹来说，是可以ls      4：表示读权限
   w:  对文件来说，是可修改文件的内容；对文件夹来说，是可以在其中创建或者删除子节点   2：表示写权限
   x： 对文件来说，是能否运行这个文件；对文件夹来说，是能否cd进入这个目录  
   第一组rwx：## 表示这个文件的拥有者对它的权限：可读可写可执行
   第二组r-x：## 表示这个文件的所属组用户对它的权限：可读，不可写，可执行
   第三组r-x：## 表示这个文件的其他用户（相对于上面两类用户）对它的权限：可读，不可写，可执行

   + chmod：修改文件的读写执行权限命令
   + chown：修改文件的所属组以及所属者，只有root权限能执行
     + -R #递归的意思
```shell
chmod +x file.txt： 给文件file.txt的所有用户加上可执行权限
chmod u-x a.sh：给自己减掉可执行权限
chmod o+rx b.sh：给其它人加两个权限
chmod g+rx b.sh：给所属组加两个权限
chmod u+x,g+w,o+w boot.log 
chmod u-x,g-w,o-w boot.log 
chmod 777 boot.log

chown angela  aaa ## 把aaa文件改变为所属用户angela
chown :angelaccc  aaa ## 把aaa文件改变为所属组angelaccc
chown angela:angelaccc aaa/ ## 同时修改所属用户和所属组
```

####  用户管理

+ 查看用户信息：cat /etc/passwd 

  /sbin/nologin #是不可登录的 
  /bin/bash #可以登录

  | 用户       | 密码占位符 | UID   | GID  | 用户描述 | 用户家目录      | 登录后使用的shell解释 |
  | ---------- | ---------- | ----- | ---- | -------- | --------------- | --------------------- |
  | testuser02 | :x         | :1010 | :502 |          | :/home/hahahome | :/bin/bash            |

+ 添加用户：useradd xiaolan 

  + -u #指定用户UID
  + -d #指定用户主目录
  + -g #指定用户所属组
  + -r #指定用户是系统用户
  + -s #用户登录shell解释器

+ 添加用户组命令：groupadd

+ 删除用户组命令：groupdel

+ 修改用户的信息命令：usermod

  + -u #指定用户UID
  + -d #指定用户主目录
  + -g #指定用户所属组

+ groups：查看当前用户组信息

+ 切换到xiaolan用户：su xiaolan 

+ id：查看用户信息

+ 给用户设置密码：passwd xiaolan 


```shell
[root@mini1 home]# passwd xiaolan 
Changing password for user xiaolan.
New password: 
BAD PASSWORD: it is too simplistic/systematic
BAD PASSWORD: is too simple
Retype new password: 
passwd: all authentication tokens updated successfully.
# 删除用户，但是前提是这个用户是处于退出状态，否则出现以下信息（可以关掉session，或者在该session输入exit命令）
[root@mini1 home]# userdel -r xiaolan 
userdel: user xiaolan is currently used by process 3977

[root@mini1 ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[root@mini1 ~]# su testuser02
[testuser02@mini1 root]$ id
uid=1010(testuser02) gid=502(testgroup) groups=502(testgroup) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[testuser02@mini1 root]$ su - root
Password: 
[root@mini1 ~]# id testuser02
uid=1010(testuser02) gid=502(testgroup) groups=502(testgroup)
```

####  普通用户进行管理员权限操作

sudo 命令

对这个进行配置：vi /etc/sudoers

```shell
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
newxiaohui      ALL=(ALL)       ALL
```

这样弄：（单单su：切换成管理员）

```shell
[xiaofeng@mini1 home]$ su - newxiaohui
Password: 
[newxiaohui@mini1 ~]$ sudo useradd createxiaohui  // 临时切换

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for newxiaohui: 
[newxiaohui@mini1 ~]$
```

###  系统服务管理 

```shell
## xxx 为 具体服务名称：sshd、iptables（防火墙）、mysql
service xxx status/stop/start/restart   

## 下面的暂时不懂了，尼玛
service --status-all   # 查看系统所有的后台服务进程
service --status-all | grep httpd  # 指定

## 配置后台服务进程的开机自启：
chkconfig httpd on  ## 让httpd服务开机自启（所有系统的启动级别）
chkconfig httpd off  ## 让httpd服务开机不要自启（所有系统的启动级别）
chkconfig --level 35 httpd on ## 指定在系统的启动级别为3和5的情况下，httpd服务自启

chkconfig --list   ## 所有
chkconfig --list | grep httpd ## 指定
```

centos7：systemctl start/stop/status/<serviceName>.service

+ firewalld：防火墙；（永久关闭防火墙：systemctl disable firewalld.service）
+ network.service：网卡

```shell
查看firewall防火墙的状态：firewall-cmd --state
查看防火墙开放端口规则：firewall-cmd --list-port
开放80端口：firewall-cmd --permanent --add-port=80/tcp （--permanent永久生效，没有此参数重启后就失效）
加载生效开放的端口：firewall-cmd --reload
查询指定端口80是否开放：firewall-cmd --query-port=80/tcp
验证80端口是否开放：
    安装telnet命令：yum -y install xinetd telnet telnet-server （确认联网状态）
    安装netstat与ifconfig命令：yum -y install net-tools（确认联网状态）
关闭80端口
firewall-cmd --remove-port=80/tcp
```

### 静态IP

网卡路径：cat /etc/sysconfig/network-scripts/ifcfg-ens......        然后再重启网卡  systemctl  restart  network.service

```shell
BOOTPROTO="static" 
IPADDR=xxx.xxx.xxx.xxx 
GATEWAY=xxx.xxx.xxx.xxx 
NETMASK=255.255.255.0 
ONBOOT="yes"
```

ping不通域名：vi /etc/resolv.conf 加上以下域名服务器解析地址 

```shell
nameserver 114.114.114.114 
nameserver 8.8.8.8 
nameserver 1.1.1.1
```

### 别名

+ 永久设置：vi /root/.bashrc
  + alias vinet='vi /etc/sysconfig/network-scripts/ifcfg-ens...' 
+ 加载使立即生效： source /root/.bashrc

###  系统启动级别管理

```shell
系统启动级别管理：输入  vi /etc/inittab

# Default runlevel. The runlevels used are:
#   0 - halt (Do NOT set initdefault to this)
#   1 - Single user mode
#   2 - Multiuser, without NFS (The same as 3, if you do not have networking)
#   3 - Full multiuser mode    没有图形界面的全功能的多用户的启动级别（通常设置为这个）
#   4 - unused
#   5 - X11    有图形界面的启动级别(VMWARE一开始进入那个有图形的界面啊)
#   6 - reboot (Do NOT set initdefault to this)
#
id:5:initdefault:  # 配置默认启动级别
```

###  netstat 

```shell
功能说明：查看网络端口的使用情况
举 例：netstat -tunlp | grep nginx
-t ：显示tcp端口
-u ：显示UDP端口
-n ：指明拒绝显示别名
-l ：指明listen的
-p ：指明显示建立相关连接的程序名
安装netstat命令：yum -y install net-tools

netstat -nltp ## 查看全部端口的监听情况
netstat -nltp | grep 3306 ## 指定
netstat -nltp | grep mysql ## 指定

[root@mini1 ~]# ps -ef | grep httpd ## 指定进程，说明httpd是一个多进程的东东
root       2020      1  0 10:11 ?        00:00:00 /usr/sbin/httpd ## 当前进程号，父进程
apache     2024   2020  0 10:11 ?        00:00:00 /usr/sbin/httpd ## 父进程是2020啊
apache     2025   2020  0 10:11 ?        00:00:00 /usr/sbin/httpd ## /usr/sbin/httpd 表示是通过这个目录下面的命令启动这个进程的
apache     2027   2020  0 10:11 ?        00:00:00 /usr/sbin/httpd
root       2310   2245  0 10:29 pts/0    00:00:00 grep httpd
```

### lsof

查看端口被占用了

```shell
lsof -i:80
```

### sar

查看CPU

```
sar -p （查看全天）
sar -u 1 10 （1：每隔一秒，10：写入10次）
```

查看内存

```
sar -r （查看全天）
sar -r 1 10 （1：每隔一秒，10：写入10次）
```

磁盘I/O

```
sar -d （查看全天）
sar -d 1 2 （1：每隔一秒，2：写入2次）
```

网络流量

```
sar -n DEV （查看全天）
sar -n DEV 1 2 （1：每隔一秒，2：写入2次）
```

###  ps

process status 的缩写

```shell
## 打印当前所有进程
ps -ef 
## 举 例：
ps -ef | grep nginx 
ps -aux | grep nginx 
ps -aux | more  ## 这样的话，就连接起来分页查看了嘛
ps -ef | more ## 这样的话，就连接起来分页查看了嘛
```

+ UID：用户ID
+ PID：进程ID
+ PPID：父进程号 
+ C：CPU的占用率 
+ STIME：进程的启动时间 
+ TTY：TTY终端 
+ TIME：进程执行起到现在总的CPU占用时间 
+ CMD：启动这个进程的命令 

```shell
[root@mini1 aaa]# ps -ef | head -n 5
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 Nov18 ?        00:00:01 /sbin/init
root          2      0  0 Nov18 ?        00:00:00 [kthreadd]
root          3      2  0 Nov18 ?        00:00:00 [migration/0]
root          4      2  0 Nov18 ?        00:00:00 [ksoftirqd/0]
```

+ USER：哪个用户启动了这个命令
+ PID：进程的ID 
+ %CPU：CPU的占用率 
+ %MEM：内存的使用率
+ VSZ：如果一个程序完全驻留在内存中一共需要使用多少内存空间 
+ RSS：进程当前占用了多少内存 
+ TTY：tty终端
+ STAT：表示当前进程的状态（S#处于休眠的状态；D#不可中断的状态 ；Z#僵尸进程 ；X#死掉的进程） 
+ START：启动这个命令的时间点
+ TIME：进程执行起到现在总的CPU占用时间 
+ COMMAND：启动这个进程的命令

```shell
[root@mini1 aaa]# ps -aux | head -n 5
Warning: bad syntax, perhaps a bogus '-'? See /usr/share/doc/procps-3.2.8/FAQ
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          1  0.0  0.0  19364  1536 ?        Ss   Nov18   0:01 /sbin/init
root          2  0.0  0.0      0     0 ?        S    Nov18   0:00 [kthreadd]
root          3  0.0  0.0      0     0 ?        S    Nov18   0:00 [migration/0]
root          4  0.0  0.0      0     0 ?        S    Nov18   0:00 [ksoftirqd/0]
```

###  free;top;df;du

top ：监控Linux系统状况，比如cpu、内存的使用；按住键盘q退出

```shell
[root@SYSOPS00074145 gxf]# free -h  ## 查看系统内存
              total        used        free      shared  buff/cache   available
Mem:           3.7G        1.6G        230M        184M        1.9G        1.7G
Swap:          8.0G        110M        7.9G
[root@SYSOPS00074145 gxf]# free -m   ## 查看系统内存，跟上面的单位不一样
              total        used        free      shared  buff/cache   available
Mem:           3790        1589         230         184        1970        1709
Swap:          8191         110        8081

(df 查看整体的哦)

[root@SYSOPS00074145 practice]# df -h ## 文件系统的磁盘使用情况统计 
[root@mini1 mypractice]# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/vg_mini1-lv_root
                       18G  7.1G  9.3G  44% /
tmpfs                 1.5G  432K  1.5G   1% /dev/shm
/dev/sda1             477M   37M  415M   9% /boot
/dev/sr0              3.7G  3.7G     0 100% /media/CentOS_6.7_Final


(du 查看细节的哦)
[root@mini1 home]# ll
total 16
drwxr-xr-x. 2 root       root       4096 Jul 22 23:44 02xiaofeng
-rw-r--r--. 1 root       root          0 Jul 30 23:43 filehaha.txt
drwxr-xr-x. 4 root       root       4096 Jul 22 23:44 haha
drwx------. 5 guoxiaohui guoxiaohui 4096 Jul 28 13:35 xiaofeng
drwxr-xr-x. 2 root       root       4096 Jul 22 23:44 xixi
[root@mini1 home]# du -sh *  ## 查看当前文件下所有文件夹以及文件的大小
4.0K    02xiaofeng
0       filehaha.txt
20K     haha
254M    xiaofeng
4.0K    xixi
```

###  uniq;sort

```shell
uniq命令：对排序好的内容进行统计
sort命令：对内容进行排序
举 例：uniq -c 123.txt | sort -n
```

###  父子进程;管道

```shell
## 父进程是：35026，子进程是：46478
[root@mini1 mypractice]# echo $$
35026
[root@mini1 mypractice]# /bin/bash
[root@mini1 mypractice]# echo $$
46478
[root@mini1 mypractice]# pstree
......
├─sshd───sshd───bash───bash───pstree
......
[root@mini1 mypractice]# ps -ef | grep 46478
root      46478  35026  0 04:09 pts/3    00:00:00 /bin/bash
root      46601  46478  0 04:10 pts/3    00:00:00 ps -ef
root      46602  46478  0 04:10 pts/3    00:00:00 grep 46478
[root@mini1 mypractice]# exit
exit
[root@mini1 mypractice]# echo $$
35026

## 父子进程本来是隔离的，export之后子进程就可以读到父进程的东东了
[root@mini1 mypractice]# x=100
[root@mini1 mypractice]# echo $x
100
[root@mini1 mypractice]# /bin/bash
[root@mini1 mypractice]# echo $x

[root@mini1 mypractice]# exit
exit
[root@mini1 mypractice]# export x
[root@mini1 mypractice]# /bin/bash
[root@mini1 mypractice]# echo $x
100
[root@mini1 mypractice]#


## 管道机制，当是管道的时候，分别在两边启动一个子进程，“a=99 ; echo "affsfsdfg";”都是在子进程里面，"affsfsdfg" 通过管道输出去了，子进程结束之后，a还是111
[root@mini1 mypractice]# echo $$
46896
[root@mini1 mypractice]# a=111
[root@mini1 mypractice]# echo $a
111
[root@mini1 mypractice]# { a=99 ; echo "affsfsdfg"; } | cat
affsfsdfg
[root@mini1 mypractice]# echo $a
111
[root@mini1 mypractice]# echo $$
46896
## echo $$ 以及 echo $BASHPID 都可以打印当前进程的id号，但是echo $$的优先级高于管道，echo $BASHPID的优先级低于管道
[root@mini1 mypractice]# echo $$
46896
[root@mini1 mypractice]# echo $$ | cat
46896
[root@mini1 mypractice]# echo $BASHPID | cat
47414

## 管道
## 产生子进程号 43378
[root@mini1 mypractice]# { echo $BASHPID ; read x;} | { cat ; echo $BASHPID ; read y;  }
47738
...（阻塞当中）
## 父进程 46896 有两个子进程 47738  47739 （就是上面管道两边的子进程嘛）
[root@mini1 ~]# ps -ef | grep 46896
root      46896  35026  0 04:16 pts/3    00:00:00 /bin/bash
root      47738  46896  0 04:39 pts/3    00:00:00 /bin/bash
root      47739  46896  0 04:39 pts/3    00:00:00 /bin/bash
root      47876  47741  0 04:40 pts/4    00:00:00 grep 46896
[root@mini1 47738]# cd /proc/47738/fd
## 有没有看到了文件描述符 1 
[root@mini1 fd]# ll
total 0
lrwx------. 1 root root 64 May 17 04:42 0 -> /dev/pts/3
l-wx------. 1 root root 64 May 17 04:42 1 -> pipe:[357846]
lrwx------. 1 root root 64 May 17 04:40 2 -> /dev/pts/3
lrwx------. 1 root root 64 May 17 04:42 255 -> /dev/pts/3
lrwx------. 1 root root 64 May 17 04:42 6 -> /dev/pts/3
## 有没有看到了文件描述符 2
[root@mini1 fd]# cd /proc/47739/fd
[root@mini1 fd]# ll
total 0
lr-x------. 1 root root 64 May 17 04:44 0 -> pipe:[357846]
lrwx------. 1 root root 64 May 17 04:44 1 -> /dev/pts/3
lrwx------. 1 root root 64 May 17 04:40 2 -> /dev/pts/3
lrwx------. 1 root root 64 May 17 04:44 255 -> /dev/pts/3
lrwx------. 1 root root 64 May 17 04:44 6 -> /dev/pts/3
## ## 有没有看到了文件描述符 1  FIFO pipe
[root@mini1 fd]# lsof -op 47738
COMMAND   PID USER   FD   TYPE DEVICE OFFSET    NODE NAME
bash    47738 root  cwd    DIR  253,0        1048592 /root/mypractice
bash    47738 root  rtd    DIR  253,0              2 /
bash    47738 root  txt    REG  253,0         924520 /bin/bash
bash    47738 root  mem    REG  253,0         264864 /lib64/ld-2.12.so
bash    47738 root  mem    REG  253,0         264866 /lib64/libdl-2.12.so
bash    47738 root  mem    REG  253,0         264865 /lib64/libc-2.12.so
bash    47738 root  mem    REG  253,0         264899 /lib64/libtinfo.so.5.7
bash    47738 root  mem    REG  253,0         261668 /lib64/libnss_files-2.12.so
bash    47738 root  mem    REG  253,0         656665 /usr/lib/locale/locale-archive
bash    47738 root  mem    REG  253,0         656925 /usr/lib64/gconv/gconv-modules.cache
bash    47738 root    0u   CHR  136,3    0t0       6 /dev/pts/3
bash    47738 root    1w  FIFO    0,8    0t0  357846 pipe
bash    47738 root    2u   CHR  136,3    0t0       6 /dev/pts/3
bash    47738 root    6u   CHR  136,3    0t0       6 /dev/pts/3
bash    47738 root  255u   CHR  136,3    0t0       6 /dev/pts/3
## 有没有看到了文件描述符 2 FIFO pipe
[root@mini1 fd]# lsof -op 47739
COMMAND   PID USER   FD   TYPE DEVICE OFFSET    NODE NAME
bash    47739 root  cwd    DIR  253,0        1048592 /root/mypractice
bash    47739 root  rtd    DIR  253,0              2 /
bash    47739 root  txt    REG  253,0         924520 /bin/bash
bash    47739 root  mem    REG  253,0         264864 /lib64/ld-2.12.so
bash    47739 root  mem    REG  253,0         264866 /lib64/libdl-2.12.so
bash    47739 root  mem    REG  253,0         264865 /lib64/libc-2.12.so
bash    47739 root  mem    REG  253,0         264899 /lib64/libtinfo.so.5.7
bash    47739 root  mem    REG  253,0         261668 /lib64/libnss_files-2.12.so
bash    47739 root  mem    REG  253,0         656665 /usr/lib/locale/locale-archive
bash    47739 root  mem    REG  253,0         656925 /usr/lib64/gconv/gconv-modules.cache
bash    47739 root    0r  FIFO    0,8    0t0  357846 pipe
bash    47739 root    1u   CHR  136,3    0t0       6 /dev/pts/3
bash    47739 root    2u   CHR  136,3    0t0       6 /dev/pts/3
bash    47739 root    6u   CHR  136,3    0t0       6 /dev/pts/3
bash    47739 root  255u   CHR  136,3    0t0       6 /dev/pts/3
```

###  挂载

```shell
可以挂载光盘、硬盘、磁带、光盘镜像文件等
1/ 挂载光驱
mkdir   /mnt/cdrom      创建一个目录，用来挂载
mount -t iso9660 -o ro /dev/cdrom /mnt/cdrom/     将设备/dev/cdrom挂载到 挂载点 ：  /mnt/cdrom中
2/ 挂载光盘镜像文件（.iso文件）
mount -t iso9660 -o loop  /home/hadoop/Centos-6.7.DVD.iso /mnt/centos
3/ 卸载 umount
umount /mnt/cdrom
```

###  网络管理

主机名配置

```shell
1)查看主机名：hostname
2）修改主机名(重启后无效)：hostname hadoop
3）修改主机名(重启后永久生效)：vi /etc/sysconfig/network
```

IP地址配置，修改IP地址：

```shell
1）方式一（用root输入setup命令，进入交互式修改界面，并非通用设置）：setup
2）方式二：修改配置文件 (重启后永久生效，万能方式，重要)： vi /etc/sysconfig/network-scripts/ifcfg-eth0 
3）方式三：ifconfig命令 (重启后无效)： ifconfig eth0 192.168.12.22

[root@mini1 ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0 
DEVICE=eth0   #设备名称
HWADDR=00:0c:29:9d:cd:ae #硬件地址，不要轻易动，如果是克隆的话，注意要保持一致（NAT 的 MAC ADDRESS）
TYPE=Ethernet   #网络类型
UUID=1c1ad075-3ef3-417e-b386-e26ee88ad1c6 #这个可以不要
ONBOOT=yes # 主要将no设置为yes，启动虚拟机的时候，就可以立刻连接
NM_CONTROLLED=yes
BOOTPROTO=none   #这个可以不要
IPADDR=192.168.174.20 # IP地址 （主要加这个）
NETMASK=255.255.255.0 # 子掩码，好像是固定的（主要加这个）
GATEWAY=192.168.174.2 # 网关（主要加这个）
DNS1=192.168.174.2  # 一般跟网关一样（主要加这个）
IPV6INIT=no  #这个可以不要
USERCTL=no   #这个可以不要
```

###  域名映射

vi /etc/hosts

配置完后最好重启：reboot

查看机器名称：hostname

```shell
[root@mini1 ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.174.20   mini1
[root@mini1 ~]# ping 192.168.174.20
[root@mini1 ~]# ping mini1
```

###  免密登录

**注意：自己也要给自己配置免密登录哦**

```shell
ssh-keygen -t rsa   ## 生成公钥和秘钥
ssh-copy-id root@mini2  ##  把钥匙发送到mini2这台机器上面
reboot ## 重启才能使配置生效
ssh mini2;ssh 192.168.207.133  ##  登录另外一台机器，免密登录？
exit ## 退出当前登录的机器
```

```shell
[root@centos-7 ~]# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
/root/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.   ## 秘钥生成在这里了
The key fingerprint is:
SHA256:5AO6rUBErNylqmQL0WUjMbidc6YWrDTfHvubD66Lbwo root@centos-7
The key's randomart image is:
+---[RSA 2048]----+
| o+.             |
|..o.+.           |
|.*o=o.. .        |
|+=Boo. +         |
|.o=*o   S        |
|o=o. =   .       |
|=Eo o +.         |
|.....=. o        |
|   o=++=o.       |
+----[SHA256]-----+
[root@centos-7 ~]# ll /root/.ssh
total 12
-rw-------. 1 root root 1675 Dec 20 22:55 id_rsa
-rw-r--r--. 1 root root  395 Dec 20 22:55 id_rsa.pub
-rw-r--r--. 1 root root  403 Dec 20 22:45 known_hosts
## 这里拷贝的另外一种很麻烦的方法，注意拷贝过去的名字一定要是 authorized_keys
[root@centos-7 ~]# scp /root/.ssh/id_rsa.pub root@mini1:/root/.ssh/authorized_keys
root@mini1's password: 
id_rsa.pub                                                 100%  395   747.5KB/s   00:00    
[root@centos-7 ~]# reboot
```

### 数据库

```shell

192.168.8.61，192.168.8.62  root/cxtest2017

~richmail/bin/mysql_rm

show databases;

drop database monitor1;

create database monitor1 character set utf8 collate utf8_bin;
use monitor1;
source /home/richmail/richmonitor/schema.sql;
source /home/richmail/richmonitor/images.sql;
source /home/richmail/richmonitor/data.sql;
```

```shell
#!/bin/sh

mysql_home=/home/richmail/mysql
db_user=richmail
db_port=3308
db_host=192.168.8.181
db_password=cx12345678
#db_password=cx12345678

$mysql_home/bin/mysql -P$db_port -h$db_host -u$db_user -p$db_password
```

