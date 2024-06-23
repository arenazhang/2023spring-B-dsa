# Assignment #7: April 月考

Updated 1557 GMT+8 Apr 3, 2024

2024 spring, Complied by 张惠雯，生命科学学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1





## 1. 题目

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/



思路：开一个空的倒叙列表，然后一边pop一边append即可



代码

```python
# 
sen=input().split()
n=len(sen)
anti_sen=[]
for i in range(n):
    anti_sen.append(sen.pop())
print(' '.join(map(str,anti_sen)))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240409205855137](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240409205855137.png)



### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/



思路：题目的提示里面有提到用queue，所以做起来就特别快。初始化一个规定了最大长度的deque，然后按照题目要求进行入队即可，出队是自动的。



代码

```python
# 
from collections import deque
m,n=map(int,input().split())
b=input().split()
dic=deque(maxlen=m)
ans=0
for i in range(n):
    if b[i] not in dic:
        dic.append(b[i])
        ans+=1
    else:
        continue
print(ans)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240409210051752](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240409210051752.png)



### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/



思路：把序列从小到大排序即可，需要特别注意K=0的情况



代码

```python
# 
n,k=map(int,input().split())
seq=list(map(int,input().split()))
seq.sort()
if k==0:
    if seq[0]-1>0:
        print(seq[0]-1)
    else:
        print(-1)
elif n>k and seq[k-1]!=seq[k]:


    print(seq[k-1])
elif n==k:

    print(seq[-1])

else:
    print(-1)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240409210126921](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240409210126921.png)



### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/



思路：按照题目的要求定义一个build_Tree 函数进行递归，首先确认a的类型并存储在value中，其次分为左右两个子树继续进行递归，最后再套用一个按层次遍历的函数即可。



代码

```python
# 
class Node:
    def __init__(self):
        self.value=None
        self.left=None
        self.right=None


def build_Tree(a):
    root=Node()
    if '0' in a and '1' in a:
        root.value='F'
    elif '1' in a:
        root.value='I'
    else:
        root.value='B'

    l=len(a)//2
    if l>0:
        root.left=build_Tree(a[:l])
        root.right=build_Tree(a[l:])
    return root

def post_traverse(node):
    output=[]
    if node:
        output.extend(post_traverse(node.left))
        output.extend(post_traverse(node.right))
        output.append(node.value)
    return ''.join(output)

n=int(input())
a=input()
root=build_Tree(a)
print(post_traverse(root))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240409211508666](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240409211508666.png)



### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/



思路：主要的思路是用 groups 字典来存储每个小组的队列，键是小组的 ID，值是一个双端队列，用于存储小组成员的编号和用 member_to_groups字典来映射每个成员到其所在的小组，键是成员的编号，值是所在小组的 ID（假设每个小组的第一个成员的编号代表该小组的 ID），创建一个队列queue用于存储小组的顺序和一个集合 queue_set用于快速检查小组是否已经在队列中。随后按照不同输入的操作对于queue和queue_set进行更新。



代码

```python
# 
from collections import deque				

# Initialize groups and mapping of members to their groups
t = int(input())
groups = {}
member_to_group = {}
for _ in range(t):
    members = list(map(int, input().split()))
    group_id = members[0]  # Assuming the first member's ID represents the group ID
    groups[group_id] = deque()
    for member in members:
        member_to_group[member] = group_id

# Initialize the main queue to keep track of the group order
queue = deque()
# A set to quickly check if a group is already in the queue
queue_set = set()


while True:
    command = input().split()
    if command[0] == 'STOP':
        break
    elif command[0] == 'ENQUEUE':
        x = int(command[1])
        group = member_to_group.get(x, None)
        # Create a new group if it's a new member not in the initial list
        if group is None:
            group = x
            groups[group] = deque([x])
            member_to_group[x] = group
        else:
            groups[group].append(x)
        if group not in queue_set:
            queue.append(group)
            queue_set.add(group)
    elif command[0] == 'DEQUEUE':
        if queue:
            group = queue[0]
            x = groups[group].popleft()
            print(x)
            if not groups[group]:  # If the group's queue is empty, remove it from the main queue
                queue.popleft()
                queue_set.remove(group)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240409212825471](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240409212825471.png)



### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/



思路：大概的思路是如果节点有子节点，就把子节点按升序排序，并将其与其子节点的关系记录在字典 tree 中，然后遍历字典 tree 中的每个节点，通过检查每个节点是否作为其他节点的子节点，将不是任何节点的子节点标记为根节点，并记录在字典 findroot 中。最后进行遍历，首先判断当前节点是否为叶子节点，如果是，则将该节点加入到结果列表 op 中，如果不是就遍历该节点的子节点，并将结果添加到 op 中。



代码

```python
# 
tree = {}
findroot = {}
n = int(input())
for i in range(n):
    l = list(map(int,input().split()))
    if len(l) > 1:
        root, child = l[0], l[1:]
        tree[root] = sorted(child)
        findroot[root] = True
    else:
        tree[l[0]] = []
for k in list(findroot.keys()):
    for j in list(findroot.keys()):
        for i in tree[j]:
            if k == i:
                findroot[k] = False
                break
for k in findroot.keys():
    if findroot[k] == True:
        root0 = k
        break
def traversal(i):
    op = []
    judge = 0
    if tree[i] == []:
        op.append(i)
    else:
        for x in tree[i]:
            if x > i and judge == 0:
                op.append(i)
                judge = 1
            op.extend(traversal(x))
        if judge == 0:
            op.append(i)
    return(op)
ans = traversal(root0)
for i in ans:
    print(i)
```



代码运行截图 

![image-20240409205104630](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240409205104630.png)



## 2. 学习总结和收获

因为这周发烧了，卧床休息了三四天，所以花在数算上的时间不太多。前四题都比较顺利写出来了，后面两题遇到了一些思路上的困难，也没有花费太多时间就去看了题解问了同学，把代码看懂就是胜利。等到期中季之后会回来再重新自己尝试写一下最后两题。



