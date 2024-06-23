# Assignment #F: All-Killed 满分

Updated 1844 GMT+8 May 20, 2024

2024 spring, Complied by 生命科学学院，张惠雯



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1





## 1. 题目

### 22485: 升空的焰火，从侧面看

http://cs101.openjudge.cn/practice/22485/



思路：

首先就是用一个数组储存所有的节点，然后按要求更新节点的关系即可。在BFS的部分使用了两个队列来回互换的方法，这样可以很方便省去判断层数的问题（一开始只有一个队列的时候还要考虑循环，什么时候遇到该层最后一个节点），只要这个q空了，那么最后一个node一定就是最右侧的节点。

自我感觉这个思路还蛮巧的hh

代码

```python
# 2300012302 张惠雯
class Node:
    def __init__(self,v):
        self.left=None
        self.right=None
        self.val=v
        self.level=None

n=int(input())
from collections import deque
tree=[Node(i) for i in range(1,n+1)]

for i in range(n):
    a,b=map(int,input().split())
    if a!=-1:
        tree[i].left=tree[a-1]
    if b!=-1:
        tree[i].right=tree[b-1]
q1=deque([tree[0]])
q2=deque([])
visited=0
output=[]
while visited!=n:
    if len(q2)==0:
        while q1:
            node=q1.popleft()
            visited+=1
            if node.left:
                q2.append(node.left)
            if node.right:
                q2.append(node.right)
            if len(q1)==0:
                output.append(str(node.val))
    if len(q1)==0:
        while q2:
            node=q2.popleft()
            visited+=1
            if node.left:
                q1.append(node.left)
            if node.right:
                q1.append(node.right)
            if len(q2)==0:
                output.append(str(node.val))
print(' '.join(output))








```



代码运行截图

![image-20240527094641388](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240527094641388.png)





### 28203:【模板】单调栈

http://cs101.openjudge.cn/practice/28203/



思路：是经典的模板题，但是太久没写栈有点生疏了，还是回去看了一下单调栈的概念才写出来。



代码

```python
# 
n=int(input())
a=list(map(int,input().split()))


def monotonic_stack(nums):
    stack = []
    result = [str(0)] * len(nums)

    for i in range(len(nums)):
        # 当栈非空且当前元素比栈顶元素大时，出栈并更新结果
        while stack and nums[i] > nums[stack[-1]]:
            result[stack.pop()] = str(i+1)

        # 将当前元素的索引入栈
        stack.append(i)

    return result
print(' '.join(monotonic_stack(a)))




```



代码运行截图 

![image-20240527101406716](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240527101406716.png)



### 09202: 舰队、海域出击！

http://cs101.openjudge.cn/practice/09202/



思路：自己写的时候不知道为什么一直WA，样例能过，自己构造的例子也都能过，所以直接参考了群里同学的解法



代码

```python
# 
from collections import deque
for _ in range(T:=int(input())):
    N,M=map(int,input().split())
    # 邻接表，入度 
    graph={i:[] for i in range(1,N+1)}
    in_degree=[0 for i in range(N+1)]
    # from,to
    for i in range(M):
        f,t=map(int,input().split())
        graph[f].append(t)
        in_degree[t]+=1

    q,cnt,visited=deque(),0,set()
    for i in range(1,N+1):
        if in_degree[i]==0:
            q.append(i)
    while q:
        cur=q.popleft()
        cnt+=1
        for vert in graph[cur]:
            in_degree[vert]-=1
            if in_degree[vert]==0 and vert not in visited:
                visited.add(vert)
                q.append(vert)

    print('No' if cnt==N else "Yes")
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240527201531558](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240527201531558.png)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135/



思路：自己写的时候是不断更新和数组，然后tle了，问了GPT给了一种比较难主动想到但是很好理解的方法，也能成功AC，感觉二分查找真的很容易出现超索引的问题

首先定义了`can_allocate`函数，该函数检查是否存在一种分配方案，使得每个财政周期的开销不超过给定的值。然后，`find_min_max_expense`函数使用二分查找来找到最大的满足条件的值，即最小的最大月度开销。最后，读取输入数据并调用`find_min_max_expense`函数，输出结果。



代码

```python
# 
def can_allocate(expenses, n, m, max_expense):
    total_months = 1
    current_month_expense = 0
    
    for i in range(n):
        current_month_expense += expenses[i]
        if current_month_expense > max_expense:
            total_months += 1
            current_month_expense = expenses[i]
    
    return total_months <= m

def find_min_max_expense(expenses, n, m):
    low = max(expenses)
    high = sum(expenses)
    
    while low < high:
        mid = (low + high) // 2
        if can_allocate(expenses, n, m, mid):
            high = mid
        else:
            low = mid + 1
    
    return low

# 读取输入
N, M = map(int, input().split())
expenses = [int(input()) for _ in range(N)]

# 调用函数找到最小的最大月度开销
result = find_min_max_expense(expenses, N, M)
print(result)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240527224251764](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240527224251764.png)



### 07735: 道路

http://cs101.openjudge.cn/practice/07735/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 01182: 食物链

http://cs101.openjudge.cn/practice/01182/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

怕忙起来忘了所以先交上来

前两题还都挺顺利的，特别是第一题，感觉灵光一现就想到了两个队列来回倒的思路。三四题已经开始有点吃力了，特别是拓扑排序问了gpt也自己构造了数据都找不出问题，最后只好去看题解，debug不成功还挺难受的，二分查找也是因为太久没写了很不熟练，但是好在题解能看懂，跟着题解走了一遍也能大致明白实现原理。

最后两题好难，大体的思路都有但是根本落不到实处，一动手写就遇到这样那样的细节问题

这次作业感觉只有前两题是比较友好的：（开始担心期末机考的状态了



