# Assignment #1: 拉齐大家Python水平

Updated 0940 GMT+8 Feb 19, 2024

2024 spring, Complied by 张惠雯 生命科学学院



**说明：**

1）数算课程的先修课是计概，由于计概学习中可能使用了不同的编程语言，而数算课程要求Python语言，因此第一周作业练习Python编程。如果有同学坚持使用C/C++，也可以，但是建议也要会Python语言。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1





## 1. 题目

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/



思路：先构造一个初始的列表，然后分两种情况讨论，如果n在3以内就不需要继续枚举，如果n超过三就需要一一计算直到所需的位数。

用时：5min



##### 代码

```python
# 2300012302 张惠雯
n=int(input())
list1=[0,1,1]
if n<=3:
    print(list1[n-1])
else:
    for i in range(3,n+1):
        list1.append(list1[i-3]+list1[i-2]+list1[i-1])
    print(list1[n])

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240220154001233](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240220154001233.png)



### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A



思路：一开始使用了从前往后的遍历方法，但是写到第二重循环的时候就意识到会超时，所以想了一下决定用栈的方法从后往前pop，好处就是pop掉之后就直接删除了这个元素不需要对列表进行反复遍历。最后用一个i作为判断的指针，对结果进行判断即可。

用时：20min



##### 代码

```python
# 2300012302 张惠雯
s=input()
li=[]
for z in s:
    li.append(z)

i=0#指针
for x in range(len(s)-1,-1,-1):
    k=li.pop()
    if i==0 and k=='o':
        i=1
    elif i==1 and k=='l':
        i=2
    elif i==2 and k=='l':
        i=3
    elif i==3 and k=='e':
        i=4
    elif i==4 and k=='h':
        i=5
        break
if i==5:
    print('YES')
else:
    print('N0')






```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240220160825859](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240220160825859.png)



### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A



思路：思路比较直接，就是先把输入全换为小写，再构造一个空列表，把所有辅音字母和.都依次遍历加入列表，最后用join直接输出即可

用时：7min



##### 代码

```python
# 2300012302 张惠雯
s=input()
s=s.lower()
list1=[]
for z in s:
    if z!='a' and z!='e' and z!='i' and z!='o' and z!='u' and z!='y':
        list1.append('.')
        list1.append(z)
print(''.join(list1))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240220163135801](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240220163135801.png)



### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/



思路：很直接，先用埃氏筛写出一个素数表，包括比所给数小的所有的素数，然后遍历到所给数的一半，如果两个数都在素数表里，就直接输出。



##### 代码

```python
# 2300012302
N=int(input())


primes = []
is_prime = [True]*N
is_prime[0] = False;is_prime[1] = False
for i in range(1,N//2+1):
    if is_prime[i]:
        primes.append(i)
        for k in range(2*i,N,i): #用素数去筛掉它的倍数
            is_prime[k] = False

for i in range(2,N+1):
    if i in primes and N-i in primes and i!=int(N/2):
        print(f'{i} {N-i}')
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240220165414542](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240220165414542.png)



### 23563: 多项式时间复杂度

http://cs101.openjudge.cn/practice/23563/



思路：首先把输入的式子以+为界限分开，再对于每个分开的式子取最后的项数存入一个列表，注意到系数是0的情况要分开讨论。最后输出列表里最大的数字即可。



##### 代码

```python
# 2300012302 张惠雯
a=input().split('+')
b=[]
for x in a:
    if x[0]!='0':
        x=x.split('^')
        b.append(int(x[-1]))
    else:
        b.append(0)
print(f'n^{max(b)}')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240220170937981](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240220170937981.png)



### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/



思路：思路稍微有点复杂，一共开了三个列表和一个字典，c是用来检查是否已经有人投票过这个数字，如果没有的话就更新字典和反查数组，d是用来方便遍历和比较大小值（相当于一个key和value反过来的字典），而a作为字典就是方便d的更新。最后使用pop遍历了d中所有投票的情况加入z这个列表中，再进行输出

用时：14min



##### 代码

```python
# 2300012302 张惠雯
a=dict()#字典
c=[]#用于存储是否已经有人投票
d=[]#用于反查
b=input().split()
for i in range(len(b)):
    if b[i] not in c:
        c.append(b[i])
        a[b[i]]=1
        d.append([1,b[i]])

    else:
        a[b[i]]+=1
        d.remove([a[b[i]]-1,b[i]])
        d.append([a[b[i]],b[i]])
z=[]
m=1

for i in range(len(d)):
    x=d.pop()
    if m<x[0]:
        z=[int(x[1])]
        m=x[0]
    elif m==x[0]:
        z.append(int(x[1]))
    else:
        continue
z.sort()
for j in z:
    print(j,end=' ')


#gpt提供的代码 很简洁
from collections import defaultdict

def calculate_winner(votes):
    # 创建一个哈希表来记录每个选项的票数
    counts = defaultdict(int)

    # 遍历弹幕信息，统计每个选项的票数
    for vote in votes:
        counts[vote] += 1

    # 找到最高票数
    max_count = max(counts.values())

    # 找到得票最多的选项
    winners = [option for option, count in counts.items() if count == max_count]

    # 按编号从小到大排序得票最多的选项
    winners.sort()

    return winners

# 读取输入
input_line = input().strip().split()

# 将输入转换为整数列表
votes = list(map(int, input_line))

# 计算得票最多的选项
winners = calculate_winner(votes)

# 输出结果
print(' '.join(map(str, winners)))

#学习了Counter之后自己写的代码
a=list(map(int,input().split()))
from collections import Counter
num=Counter(a)
gra=num.most_common(len(a))
z=[]
m=gra[0][1]
for i in gra:
    if i[1]==m:
        z.append(i[0])
    else:
        break
z.sort()
for x in z:
    print(x,end=' ')




```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240220230822415](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240220230822415.png)

![image-20240221105346558](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240221105346558.png)

## 2. 学习总结和收获

这次的作业总体难度都不大，基本上看到题目就有思路，然后10分钟之内都能解决，能够给自己增长一些信心，至少python语法过了一个寒假还没有全部忘掉，复建一下还能捡起来。特别是栈的pop功能很好用，有两题都用到了。

需要复习一下字典的遍历方式和defaultdict的使用方法。





