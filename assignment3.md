# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024

2024 spring, Complied by 张惠雯，生命科学学院



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1

## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/



思路：看到题目就立刻想到是dp，联想到之前的最大上升子序列和数字三角，但是无奈太久没写过dp，手生到想不出来状态转移方程，一开始想着要从后往前更新dp数组，发现思路打结了，最后是看了群里大家的讨论才写出来的。



##### 代码

```python
# 2300012302 张惠雯
def ma(heights):
    n = len(heights)

    dp = [1] * n


    for i in range(1, n):
        for j in range(i):
            if heights[i] <= heights[j]:
                dp[i] = max(dp[i], dp[j] + 1)


    return max(dp)



k = int(input())
heights = list(map(int, input().split()))


result = ma(heights)
print(result)

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240310105232274](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240310105232274.png)



**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147



思路：一开始想到是递归，但是不太清楚每一步怎么做，所以上b站搜了一个讲解视频，看了大概五分钟就明白了，然后自己写了一下一遍就AC了！好耶！！！！



##### 代码

```python
# 
def hanoi(n,x,y,z):
    if n==2:
        print(f'{1}:{x}->{y}')
        print(f'{2}:{x}->{z}')
        print(f'{1}:{y}->{z}')
    else:
        hanoi(n-1,x,z,y)
        print(f'{n}:{x}->{z}')
        hanoi(n-1,y,x,z)
n,a,b,c=map(str,input().split())
n=int(n)
hanoi(n,a,b,c)

```



代码运行截图 ==（至少包含有"Accepted"）==



![image-20240310112045396](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240310112045396.png)

**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253



思路：自己写的思路超时了，参考了题解。个人感觉题解最巧妙的地方就是先pop再append，这样可以避免不断对索引进行更改和迭代的麻烦，思路也更加清晰，不容易出现超索引或者反复循环的情况。



##### 代码

```python
# 
while True:
    n, p, m = map(int, input().split())
    if {n,p,m} == {0}:
        break
    monkey = [i for i in range(1, n+1)]
    for _ in range(p-1):
        tmp = monkey.pop(0)
        monkey.append(tmp)
    # print(monkey)

    index = 0
    ans = []
    while len(monkey) != 1:
        temp = monkey.pop(0)
        index += 1
        if index == m:
            index = 0
            ans.append(temp)
            continue
        monkey.append(temp)

    ans.extend(monkey)

    print(','.join(map(str, ans)))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310094207656](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240310094207656.png)



**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554



思路：逻辑上就是耗时越小的排在越前面，这样可以达到贪心的目的。做法就是用字典来存储时间和人，时间是key，序号是value，为了避免有重复的key覆盖，就把value设置为列表，方便保留原有的顺序。后续就是把key取出进行排序，再通过字典的查找功能按照新的key的顺序把同学的序号排列好。最后计算一下总耗时，注意边界情况。

耗时：20min

##### 代码

```python
# 2300012302 张惠雯
n=int(input())
t=list(map(int,input().split()))
ti=dict()
for i in range(n):
    if t[i] not in ti:
        ti[t[i]]=[i]
    else:
        ti[t[i]].append(i)
a=sorted(ti)
a.sort()
se=[]
for x in a:
    for y in ti[x]:
        se.append(y+1)
ans=0
t=sorted(t)

for i in range(n-1):
    ans+=t[i]*(n-i-1)
for j in se:
    print(j,end=' ')
print()
print('{:.2f}'.format(ans/n))
```



代码运行截图 ![image-20240309204759972](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240309204759972.png)

**19963:买学区房**

http://cs101.openjudge.cn/practice/19963



思路：大概就是先接收数据，然后遍历计算每个房子的性价比，并将每个房子的性价比和价格构成一个列表加入一个大的列表中，中位数的计算注意n是总个数而不是列表的索引。做的时候一直WA没办法debug，最后发现是因为计算中位数的时候把/写成了//。。看来以后写代码的时候还是不能着急，一着急就容易出现低级错误



##### 代码

```python
# 
n=int(input())
pairs = [i[1:-1] for i in input().split()]
le = [sum(map(int,i.split(','))) for i in pairs]
price=list(map(int,input().split()))
hou=[]
ans=[]
for i in range(n):

    an=le[i]/price[i]#性价比
    ans.append(an)
    hou.append([an,price[i]])
price.sort()
ans.sort()

if n%2!=0:
    pri=price[(n-1)//2]
    anss=ans[(n-1)//2]
else:
    pri=(price[(n-2)//2]+price[n//2])/2
    anss=(ans[(n-2)//2]+ans[n//2])/2
ok=0

for j in hou:
    if j[0]>anss and j[1]<pri:
        ok+=1

print(ok)



```



代码运行截图



![image-20240309212352307](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240309212352307.png)

**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300



思路：首先就是接收输入，然后放入一个以名称为key，所有数据组合的列表为value的一个字典，然后对于字典内部进行排序和处理，最终输出。一开始一直WA，后来自己构造了测试数据发现是浮点数和整数比较的原因（直接用字典序比较会出错），全部换成浮点数输出又会有问题，所以最后干脆分了类分别处理。思路很简单，主要是格式问题比较容易忽略。



##### 代码

```python
# 2300012302 张惠雯
n=int(input())
ans=dict()
for i in range(n):
    x=input().split('-')
    if x[0] not in ans:
        ans[x[0]]=[x[1]]
    else:
        ans[x[0]].append(x[1])
keys=sorted(ans)
for j in keys:
    a=ans[j]
    B=[]
    M=[]
    for y in a:
        if y[-1]=='B':
            if float(y[0:-1])%1==0:
                B.append(int(y[0:-1]))
            else:
                B.append(float(y[0:-1]))

        else:
            if float(y[0:-1])%1==0:
                M.append(int(y[0:-1]))
            else:
                M.append(float(y[0:-1]))
    B.sort()
    M.sort()
    b=[]
    for i in M:
        b.append(str(i)+'M')
    for i in B:
        b.append(str(i)+'B')
    ans[j]=b
keys.sort()
for x in keys:
    print(f'{x}:',', '.join(ans[x]))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240309234850192](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240309234850192.png)



## 2. 学习总结和收获

这次的月考题感觉难度的区别还是挺大的，一开始从第一题卡住就跳，后面三题都比较简单，主要是会有一些小坑和细节问题，倒是都顺利做出来了。经过三周的训练，能感觉到写代码的手感回来了一点，但是对于稍微难一点的题目（比如说dp，递归之类的）还是会有畏难心理。

在周中的时候有尽量挤时间做每日选做，发现有一些题目之前做过现在捡起来还是会手生，但是经过自己努力最后AC的时候成就感爆棚！加油！





