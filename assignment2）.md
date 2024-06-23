# Assignment #2: 编程练习

Updated 0953 GMT+8 Feb 24, 2024

2024 spring, Complied by 张惠雯，生命科学学院



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1

## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/2024sp_routine/27653/



思路：因为上课的时候讲了Fraction类的具体操作方式，所以基本上写这个代码就是照葫芦画瓢，还顺便积累了欧几里得函数求最大公因数的代码到cheatsheet里面。

耗时：5min

##### 代码

```python
# 2300012302 张惠雯
class Fraction:
    def __init__(self,top,bottom):
        self.num=top
        self.den=bottom

    def __add__(self, other):
        newnum=self.num*other.den+other.num*self.den
        newden=self.den*other.den

        def gcd(m, n):
            while m % n != 0:
                oldm = m
                oldn = n
                m = oldn
                n = oldm % oldn
            return n

        a=gcd(newnum,newden)
        newnum=newnum//a
        newden=newden//a
        return Fraction(newnum,newden)


m=list(map(int,input().split()))
num1=m[0]
den1=m[1]
num2=m[2]
den2=m[3]
f1=Fraction(num1,den1)
f2=Fraction(num2,den2)
f3=f1+f2
print(f"{f3.num}/{f3.den}")

```



代码运行截图

![image-20240224220010351](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240224220010351.png)





### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110



思路：比较简单直接的思路，既然糖果可以任意组装那就不妨把每个糖果的单位价值输入一个列表中，最后按照从大往小一直取到装满为止。需要注意的有一个坑，就是有可能所有糖果的重量加起来都没有装满驯鹿，这种时候就要添加一个终止条件防止rte。

耗时：15min



##### 代码

```python
# 2300012302 张惠雯
n,w=map(int,input().split())
candy_all=[]
for i in range(n):
    a=list(map(int,input().split()))
    b=a[0]/a[1]
    for j in range(a[1]):

        candy_all.append(b)
candy_all.sort()
we=0
candy=0
while we<w and len(candy_all)>0:
    candy+=candy_all.pop()
    we+=1
if we==w:
    print(round(candy,1))
elif len(candy_all)==0:
    print(round(candy,1))




```



代码运行截图 

![image-20240226095111132](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240226095111132.png)



### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/



思路：对于每一个case，先初始化结果为alive，然后用一个字典来记录每时刻的技能量（key是时刻，技能用列表表示），最后把a的key按照时刻进行排序并输出键的列表。对于每个时刻，如果技能数小于m，就直接扣掉所有技能的血量之和，如果技能数大于m，就扣最大的m个技能的血量。

一开始想用defaultdict来初始化字典，发现会MLE，然后用了最大时间去依次在字典中判断有无技能，还是会TLE.最终学习了一下字典中输出键的列表这一方法，才成功AC。

用时：前前后后2h（）

##### 代码

```python
# 
cases=int(input())
result=[]
for i in range(cases):
    situation='alive'
    n,m,b=map(int,input().split())
    a={}
    for i in range(n):
        x,y=map(int,input().split())
        if x not in a:
            a[x]=[y]
        else:
            a[x].append(y)
    c=sorted(a)
    for i in c:
        if m >=len(a[i]):
            b-=sum(a[i])
        else:
            a[i]=sorted(a[i],reverse=True)
            b-=sum(a[i][:m])
        if b<=0:
            situation=i
            break
    print(situation)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240301005716118](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240301005716118.png)



### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B



思路：

自己写的代码已经用了线性筛了但是还是在第57个test TLE了......

只好求助题解，发现好像不打表是不可能过的，于是用回了打表的欧拉筛，打了1000000的表

然后就AC了（）

##### 代码

```python
# 2300012302
import math


def is_prime(num):
    if num <= 1:
        return False
    elif num <= 3:
        return True
    elif num % 2 == 0 or num % 3 == 0:
        return False
    i = 5
    while i * i <= num:
        if num % i == 0 or num % (i + 2) == 0:
            return False
        i += 6
    return True


n = int(input())
b = input().split()
for i in range(n):
    a = int(b[i])
    if a ** 0.5 % 1 == 0:
        if is_prime(int(a ** 0.5)):
            print('YES')
        else:
            print('NO')
    else:
        print('NO')
#自己重写的打表代码 2300012302 张惠雯
def Euler_sieve(n):
    primes = [True for _ in range(n+1)]
    p = 2
    while p*p <= n:
        if primes[p]:
            for i in range(p*p, n+1, p):
                primes[i] = False
        p += 1
    primes[0]=primes[1]=False
    return primes
a=Euler_sieve(1000000)

import math

x = int(input())
b = list(map(int, input().split()))
for i in b:

    if i<4:
        print('NO')
    elif math.sqrt(i)%1==0:
        if a[int(math.sqrt(i))]:
            print('YES')
        else:
            print('NO')
    else:
        print('NO')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240302165001480](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240302165001480.png)



### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A



思路：自己的思路是很简单的，用两个for循环遍历了所有的子序列，不出意外地超时了。

然后去看了题解，同样是双指针，题解的时间复杂度低主要是没有使用sum函数或者依次相加的方式，而是巧妙利用了反转列表的方式进行指针的移动和求和。



##### 代码

```python
# 自己的代码





t=int(input())
for z in range(t):
    n,x=map(int,input().split())
    a=list(map(int,input().split()))
    i=0
    j=n-1
    ma=0
    sm=0
    if i==j:
        if a[0]%x!=0:
            print(1)
        else:
            print(-1)
    else:
        for i in range(0,n-1):
            sm=a[i]
            for j in range(i+1,n):
                sm+=a[j]
                if sm%x!=0:
                    ma=max(ma,j-i+1)
                    if ma>=n-i:
                        break
        if ma==0:
            print(-1)
        else:
            print(ma)


#题解的代码
def prefix_sum(nums):
    prefix = []
    total = 0
    for num in nums:
        total += num
        prefix.append(total)
    return prefix
 
def suffix_sum(nums):
    suffix = []
    total = 0
    # 首先将列表反转
    reversed_nums = nums[::-1]
    for num in reversed_nums:
        total += num
        suffix.append(total)
    # 将结果反转回来
    suffix.reverse()
    return suffix
 
 
t = int(input())
for _ in range(t):
    N, x = map(int, input().split())
    a = [int(i) for i in input().split()]
    aprefix_sum = prefix_sum(a)
    asuffix_sum = suffix_sum(a)
 
    left = 0
    right = N - 1
    if right == 0:
        if a[0] % x !=0:
            print(1)
        else:
            print(-1)
        continue
 
    leftmax = 0
    rightmax = 0
    while left != right:
        total = asuffix_sum[left]
        if total % x != 0:
            leftmax = right - left + 1
            break
        else:
            left += 1
 
    left = 0
    right = N - 1
    while left != right:
        total = aprefix_sum[right]
        if total % x != 0:
            rightmax = right - left + 1
            break
        else:
            right -= 1
    
    if leftmax == 0 and rightmax == 0:
        print(-1)
    else:
        print(max(leftmax, rightmax))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240305000808016](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240305000808016.png)



### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/



思路：其实就是把Tprime的代码改了一下，然后打表的范围小一点就能过了。



##### 代码

```python
# 
def Euler_sieve(n):
    primes = [True for _ in range(n+1)]
    p = 2
    while p*p <= n:
        if primes[p]:
            for i in range(p*p, n+1, p):
                primes[i] = False
        p += 1
    primes[0]=primes[1]=False
    return primes
a=Euler_sieve(10000)

import math

m,n=map(int,input().split())
for i in range(m):
    g=list(map(int,input().split()))
    su=0
    for j in g:
        if j<4:
            continue
        elif math.sqrt(j)%1==0:
            if a[int(math.sqrt(j))]:
                su+=j
            else:
                continue
        else:
            continue
    if su==0:
        print(0)
    else:
        ans='{:.2f}'.format(su/len(g))
        print(ans)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240302170234023](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240302170234023.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。

感觉这次作业比上次难一些，主要就是卡在时间复杂度这一块，写出逻辑正确的代码并不难，但是后续不断优化的过程才是能够考验能力的部分。感觉自己上学期的数据结构部分还是不是很扎实，很多时间复杂度低的算法和思路都没有完全掌握，可能需要多积累一下。





