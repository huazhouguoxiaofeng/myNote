## cat;more;less;tail;head 

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

## 重定向

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
## 代表的是可以执行多条命令；例如这个例子，把这个东东合并输出
	[root@mini1 mypractice]# cat a1.txt 
	afsag
	agst
	gatg
	[root@mini1 mypractice]# cat a2.txt 
	2222
	[root@mini1 mypractice]# cat a1.txt ;cat a2.txt 
	afsag
	agst
	gatg
	2222
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



