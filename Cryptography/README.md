# 分组密码的差分攻击
## Author: 文火
&nbsp;&nbsp;&nbsp;&nbsp;<font size=2>逆向也算是碰到瓶颈了，打比赛除了碰到常见的显而易见的加密算法去逆运算和动态调试，其他的都是需要花时间去学的一些姿势，但是一天两天就能学成的东西提升不是很大，大三了都，积累的差不多了就需要研究点能质变的东西，所以在这一个栏目里面多加一些干货，目前这学期打算就放这个**分组密码的差分攻击**，以后在其他的栏目再放一个用**llvm理解/写编译器**和**利用angr或是miasm2控制流平坦反混淆**吧。至于pwn那儿，目前也没什么心思研究，等学会了操控IO_FILE和内核就差不多了，这学期就这么办了，这个月就研究分组密码。嗯。还是逆向有意思。</font></br>
&nbsp;&nbsp;&nbsp;&nbsp;<font size=2>所以花了一星期写了个小脚本，目前只有**生成差分表和加解密DES**的功能，加密文件的功能就不写了，我只是研究差分而已。之所以先上传就是想备份一下嘿嘿嘿，顺便有个进度的话文章每天写一点到时候也不会累死，嗯下次看来有时间还要去学一下gitbook，毕竟要是整篇文章都撂这了可读性太低。</font></br>
&nbsp;&nbsp;&nbsp;&nbsp;<font size=2>先这样吧，明天开始正式重现差分了，据说破个8轮不是问题吧哈，16轮的话比爆破都慢。</font></br>

- beta1
> 支持最简单的DES加解密与差分表的生成
- beta2
> 1.支持加密轮函数的次数指定 </br>
> 2.S-box的输入输出的demo生成
- beta3
> ...



目前进度
=======

```python

fanda@fanda:~/Desktop/crypt-analy$ python
Python 2.7.12 (default, Dec  4 2017, 14:50:18) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> from des import *
>>> a=DES()
>>> a.initDES('hello,world','01234567',2)
>>> a.show(1,1)
encrypt string(hex):
['8f33d4b2a46f974', 'fd0b89ef867b6d2']
group 0:
1 0 1 0 0 1 0 0 0 1 0 0 1 1 1 0 0 1 1 1 0 1 0 1 0 1 1 0 0 0 1 1 0 1 0 1 1 1 0 1 0 0 1 1 1 0 1 0 0 0 1 0 0 0 0 1 0 1 1 0 1 0 1 1
group 1:
0 1 1 0 1 0 0 1 0 0 1 0 0 0 0 1 0 1 0 1 1 0 0 1 1 1 1 0 1 1 1 1 0 0 0 1 1 1 0 1 1 1 1 0 1 0 0 1 0 1 1 0 1 1 1 1 0 0 1 1 1 0 1 0
the key:
K1:	0 1 0 1 0 0 0 0 0 0 1 0 1 1 0 0 1 0 1 0 1 1 0 0 0 1 0 1 0 1 0 0 0 0 1 0 0 0 1 1 0 1 0 0 0 1 1 1
K2:	0 1 0 1 0 0 0 0 1 0 1 0 1 1 0 0 1 0 1 0 0 1 0 0 0 1 1 1 0 1 0 0 1 0 0 0 0 0 0 1 0 1 0 0 0 1 0 1
K3:	1 1 0 1 0 0 0 0 1 0 1 0 1 1 0 0 0 0 1 0 0 1 1 0 0 1 0 0 0 0 1 0 1 0 1 0 0 1 0 0 1 1 0 0 1 1 1 0
K4:	1 1 1 0 0 0 0 0 1 0 1 0 0 1 1 0 0 0 1 0 0 1 1 0 0 1 1 0 1 1 0 0 1 0 1 1 0 1 0 1 1 0 0 0 1 0 0 1
K5:	1 1 1 0 0 0 0 0 1 0 0 1 0 1 1 0 0 0 1 0 0 1 1 0 0 0 1 0 1 0 1 0 0 1 0 1 0 1 0 0 0 1 1 0 1 0 1 1
K6:	1 1 1 0 0 0 0 0 1 0 0 1 0 0 1 0 0 1 1 1 0 0 1 0 0 1 0 0 1 1 1 0 1 1 0 1 1 0 0 1 0 0 1 0 0 0 1 0
K7:	1 0 1 0 0 1 0 0 1 1 0 1 0 0 1 0 0 1 1 1 0 0 1 0 1 0 0 0 0 1 0 0 0 1 0 0 1 1 0 1 0 1 1 1 1 0 0 0
K8:	1 0 1 0 0 1 1 0 0 1 0 1 0 0 1 1 0 1 0 1 0 0 1 0 1 1 0 0 1 0 0 1 1 0 0 1 1 0 1 0 0 1 0 1 0 0 0 0
K9:	0 0 1 0 0 1 1 0 0 1 0 1 0 0 1 1 0 1 0 1 0 0 1 1 1 1 0 0 1 0 0 1 1 0 0 0 0 0 1 0 0 1 1 1 1 0 0 0
K10:	0 0 1 0 1 1 1 1 0 1 0 1 0 0 0 1 0 1 0 1 0 0 0 1 1 0 0 1 0 0 0 1 1 1 0 1 1 1 1 0 0 0 0 0 1 1 0 0
K11:	0 0 0 0 1 1 1 1 0 1 0 0 0 0 0 1 1 1 0 1 1 0 0 1 0 0 0 1 1 0 0 0 0 0 0 1 0 1 1 0 1 0 1 1 0 1 0 0
K12:	0 0 0 1 1 1 1 1 0 1 0 0 0 0 0 1 1 0 0 1 1 0 0 1 1 0 0 1 1 0 0 1 0 1 1 0 1 0 0 0 1 0 1 0 0 1 0 1
K13:	0 0 0 1 1 1 1 1 0 0 0 0 1 0 0 1 1 0 0 0 1 0 0 1 0 0 1 0 0 0 1 0 0 1 1 0 1 0 1 0 1 0 0 1 0 0 0 1
K14:	0 0 0 1 1 0 1 1 0 0 1 0 1 0 0 0 1 0 0 0 1 1 0 1 1 0 1 1 0 0 1 1 0 0 1 0 0 0 0 1 0 0 0 1 0 1 1 1
K15:	0 0 0 1 1 0 0 1 0 0 1 0 1 1 0 0 1 0 0 0 1 1 0 0 1 0 1 0 0 1 1 1 0 0 0 0 0 0 1 1 1 0 0 0 0 0 1 0
K16:	0 1 0 1 0 0 0 1 0 0 1 0 1 1 0 0 1 0 0 0 1 1 0 0 0 0 0 1 0 1 1 1 0 0 1 0 0 0 1 1 1 1 0 0 0 0 1 0

Group 1:
round 1:
input: 							output:
0 1 0 1 0 0 	------------->	0 1 1 0
0 0 0 0 1 1 	------------->	1 1 0 1
1 0 1 1 0 1 	------------->	1 0 0 0
0 1 0 0 1 1 	------------->	0 1 1 1
1 0 0 0 1 0 	------------->	0 0 1 0
1 1 1 0 0 1 	------------->	0 1 1 0
1 1 0 1 1 1 	------------->	1 1 1 1
1 0 0 1 1 1 	------------->	0 1 1 1

round 2:
input: 							output:
1 1 1 1 1 1 	------------->	1 1 0 1
1 1 0 0 0 0 	------------->	0 1 0 1
0 1 0 1 0 1 	------------->	0 1 0 1
0 1 0 0 0 0 	------------->	0 0 0 1
0 1 1 0 0 1 	------------->	0 0 1 1
0 0 1 0 1 0 	------------->	0 0 1 0
1 0 1 0 0 0 	------------->	1 1 0 0
0 1 0 0 1 1 	------------->	0 1 0 1

Group 2:
round 1:
input: 							output:
1 1 0 1 0 0 	------------->	1 0 0 1
0 0 0 0 1 1 	------------->	1 1 0 1
1 0 1 1 0 1 	------------->	1 0 0 0
0 1 0 0 1 0 	------------->	0 0 1 0
1 1 0 1 0 1 	------------->	0 0 0 0
0 0 0 1 1 0 	------------->	1 1 1 1
0 0 1 1 0 1 	------------->	0 0 0 1
0 0 0 1 0 1 	------------->	1 1 0 1

round 2:
input: 							output:
0 1 0 1 1 1 	------------->	1 0 1 1
1 1 0 0 0 1 	------------->	1 0 1 1
0 0 1 1 1 1 	------------->	1 0 1 0
1 1 0 1 1 0 	------------->	1 1 1 0
1 1 0 0 0 0 	------------->	1 1 1 1
0 1 0 1 1 0 	------------->	0 1 0 0
1 0 0 0 1 0 	------------->	0 1 0 0
1 1 0 0 0 1 	------------->	1 1 1 1



decrypt string(hex):
['68656c6c6f2c776f', '726c642020202020']
group 0:
0 1 1 0 1 0 0 0 0 1 1 0 0 1 0 1 0 1 1 0 1 1 0 0 0 1 1 0 1 1 0 0 0 1 1 0 1 1 1 1 0 0 1 0 1 1 0 0 0 1 1 1 0 1 1 1 0 1 1 0 1 1 1 1
group 1:
0 1 1 1 0 0 1 0 0 1 1 0 1 1 0 0 0 1 1 0 0 1 0 0 0 0 1 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 1 0 0 0 0 0

decrypt string:
hello,world     
>>> 


```