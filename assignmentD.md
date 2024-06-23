# Assignment #D: May月考

Updated 1654 GMT+8 May 8, 2024

2024 spring, Complied by 张惠雯，生命科学学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1



## 1. 题目

### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：



代码

```python
# 
l,m=map(int,input().split())
tree=[1]*(l+1)
for i in range(m):
    x,y=map(int,input().split())
    for j in range(x,y+1):
        tree[j]=0
print(tree.count(1))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240512100243847](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240512100243847.png)



### 20449: 是否被5整除

http://cs101.openjudge.cn/practice/20449/



思路：



代码

```python
# 
a = input()
ans = []
for j in range(1, len(a) + 1):
    num = a[0:j]
    numm = int(num, 2)

    if numm % 5 == 0:
        ans.append('1')
    elif numm % 5 != 0:
        ans.append('0')

print(''.join(ans))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240512100331839](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240512100331839.png)



### 01258: Agri-Net

http://cs101.openjudge.cn/practice/01258/



思路：之前学了prim算法，但是没有成功完全靠自己实现过，作业里那题也只是看懂了题解。考试的时候想着还是试一下自己写，于是按照经典的思路写了一遍，虽然不是特别标准，但是居然能够AC，特别开心



代码

```python
# 
class Node:
    def __init__(self,v):
        self.value=v
        self.joint=set()
        self.way=dict()

def prim(x,num,al,ans,farm):
    visited=set()
    visited.add(x)
    min_way=[100000]*num
    while al!=num:
        for a in range(numm):
            if min_way[a]!=200000:

                for b in visited:
                    

                    if farm[a] in b.joint:

                        min_way[a]=min(min_way[a],b.way[farm[a]])#更新最小距离

        
        shortest=min(min_way)
        y=min_way.index(shortest)
        ans+=shortest
        visited.add(farm[y])

        min_way[y]=200000



        al+=1

    if al==num:
        return ans



while True:
    try:
        numm=int(input())
        farm=[Node(i) for i in range(numm)]
        maps=[]
        ans=[]
        for i in range(numm):
            maps.append(list(map(int,input().split())))
        for x in range(numm):
            for y in range(numm):
                farm[x].joint.add(farm[y])
                farm[x].way[farm[y]]=maps[x][y]
                farm[y].joint.add(farm[x])
                farm[y].way[farm[x]] = maps[x][y]

        ans=prim(farm[0],numm,0,0,farm)
        print(ans)







    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240512100541183](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240512100541183.png)



### 27635: 判断无向图是否连通有无回路(同23163)

http://cs101.openjudge.cn/practice/27635/



思路：判断连通的思路比较直接，判断回路还需要考虑有可能有多个连通块所以对于每个节点都要进行是否有环的判断，判断的大概思路就是如果它的邻接节点已经被访问过且不是它的上一个节点，那么就说明有环存在。

变量名真的要好好写。。机考的时候一直WA，课后debug一个小时看不出逻辑有任何问题，最后发现是有个变量名重复了导致的，，，好难过



代码

```python
class Node:
    def __init__(self, v):
        self.value = v
        self.joint = set()

def connected(x, visited, num):
    visited.add(x)
    al = 1
    q = [x]
    while al != num and q:
        x = q.pop(0)
        for y in x.joint:
            if y not in visited:
                visited.add(y)
                al += 1
                q.append(y)
    return al == num

def loop(x, visited, parent):
    visited.add(x)
    if x.joint:
        for a in x.joint:
            if a in visited and a != parent:
                return True
            elif a != parent and loop(a, visited, x):
                return True

    return False

n, m = map(int, input().split())

vertex = [Node(i) for i in range(n)]
for i in range(m):
    a, b = map(int, input().split())
    vertex[a].joint.add(vertex[b])
    vertex[b].joint.add(vertex[a])

if connected(vertex[0], set(), n):
    print('connected:yes')
else:
    print('connected:no')
x=0
for i in range(n):
    if loop(vertex[i],set(),None):
        print('loop:yes')
        x=1
        break
if x==0:
    print('loop:no')

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240512112117352](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240512112117352.png)





### 27947: 动态中位数

http://cs101.openjudge.cn/practice/27947/



思路：一开始本来想着直接维护一个堆就可以了，发现样例都过不了，原因是堆的实现原理决定了不能保证一定按照大小顺序排列，有可能出现前后几个位置的调换，于是去参考了题解，发现是需要同时维护大小两个堆，后续的操作都不难理解，就是大小堆这个思路自己想不到）已经积累到cheatsheet上了



代码

```python
# 
import heapq

def dynamic_median(nums):
    # 维护小根和大根堆（对顶），保持中位数在大根堆的顶部
    min_heap = []  # 存储较大的一半元素，使用最小堆
    max_heap = []  # 存储较小的一半元素，使用最大堆

    median = []
    for i, num in enumerate(nums):
        # 根据当前元素的大小将其插入到对应的堆中
        if not max_heap or num <= -max_heap[0]:
            heapq.heappush(max_heap, -num)
        else:
            heapq.heappush(min_heap, num)

        # 调整两个堆的大小差，使其不超过 1
        if len(max_heap) - len(min_heap) > 1:
            heapq.heappush(min_heap, -heapq.heappop(max_heap))
        elif len(min_heap) > len(max_heap):
            heapq.heappush(max_heap, -heapq.heappop(min_heap))

        if i % 2 == 0:
            median.append(-max_heap[0])

    return median

T = int(input())
for _ in range(T):
    #M = int(input())
    nums = list(map(int, input().split()))
    median = dynamic_median(nums)
    print(len(median))
    print(*median)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240512131255877](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240512131255877.png)



### 28190: 奶牛排队

http://cs101.openjudge.cn/practice/28190/



思路：看到单调栈的tag一开始想着直接用一个单调栈，但是仔细读题之后发现中间的元素只需要不相等即可，如果对于每个元素都进行遍历则时间复杂度不符合。题解中先通过预处理找到合法的A和B，再通过对每个B枚举找合适A然后更新ans。



代码

```python
# 
N = int(input())
heights = [int(input()) for _ in range(N)]

left_bound = [-1] * N
right_bound = [N] * N

stack = []  # 单调栈，存储索引

# 求左侧第一个≥h[i]的奶牛位置
for i in range(N):
    while stack and heights[stack[-1]] < heights[i]:
        stack.pop()

    if stack:
        left_bound[i] = stack[-1]

    stack.append(i)

stack = []  # 清空栈以供寻找右边界使用

# 求右侧第一个≤h[i]的奶牛位
for i in range(N-1, -1, -1):
    while stack and heights[stack[-1]] > heights[i]:
        stack.pop()

    if stack:
        right_bound[i] = stack[-1]

    stack.append(i)

ans = 0


for i in range(N):  # 枚举右端点 B寻找 A，更新 ans
    for j in range(left_bound[i] + 1, i):
        if right_bound[j] > i:
            ans = max(ans, i - j + 1)
            break
print(ans)
```



代码运行截图 

![image-20240514120740684](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240514120740684.png)



## 2. 学习总结和收获

月考的前四题基本上都能够靠自己做出来，后面两题因为时间不够在考场上只是读了一边题目，课下也没能够自己做出来，感觉一方面是对于单调栈和堆的掌握不够熟练，另一方面是有时候面对难题很难有思路。希望能至少保住前四题吧，这两周多复习一下笔试的题目。



