# Assignment #8: 图论：概念、遍历，及 树算

Updated 1919 GMT+8 Apr 8, 2024

2024 spring, Complied by 张惠雯，生命科学学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1



## 1. 题目

### 19943: 图的拉普拉斯矩阵

matrices, http://cs101.openjudge.cn/practice/19943/

请定义Vertex类，Graph类，然后实现



思路：花了接近一个小时看着定义自己手搓了vertex和graph的类，虽然可能不是最简洁的写法，但是能AC已经很开心了！大概思路就是先按照输入构建图，然后再依次构建D矩阵和A矩阵



代码

```python
# 
from collections import defaultdict


class Vertex:
    def __init__(self, value):
        self.value = value
        self.adjacent = defaultdict(int)


class Graph:
    def __init__(self):
        self.ver = {}
        self.value = []

    def add_vertex(self, vert):
        self.ver[vert.value] = vert
        self.value.append(vert.value)

    def get_vert(self, vert):
        return self.ver.get(vert, None)

    def add_edge(self, from_vert, to_vert, weight):
        self.ver[from_vert].adjacent[to_vert] = weight
        self.ver[to_vert].adjacent[from_vert] = weight


n, m = map(int, input().split())
g = Graph()

for _ in range(m):
    a, b = map(int, input().split())

    if a not in g.value:
        g.add_vertex(Vertex(a))

    if b not in g.value:
        g.add_vertex(Vertex(b))

    g.add_edge(a, b, 1)

# 构建D矩阵和A矩阵
D = [[0] * n for _ in range(n)]
A = [[0] * n for _ in range(n)]

for i, vert in g.ver.items():
    D[i][i] = len(vert.adjacent)
    for j in vert.adjacent:
        A[i][j] = 1

# 构建拉普拉斯矩阵L
L = [[D[i][j] - A[i][j] for j in range(n)] for i in range(n)]


for row in L:
    print(*row)

```



代码运行截图 

![image-20240415204010725](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240415204010725.png)



### 18160: 最大连通域面积

matrix/dfs similar, http://cs101.openjudge.cn/practice/18160



思路：很久没有写DFS，发现相关的知识点忘得差不多了，一开始尝试自己写了一个递归函数，但是不出意料地超出递归次数了，只好回去复习了一下计概当时学dfs的笔记，然后参考了笔记自己写了一下。最核心的应该就是使用queue，可以实现一路往下找的过程，最后返回母节点。

代码

```python
# 
t = int(input())
result = []
for _ in range(t):
    re = []
    ans = 0
    N, M = map(int, input().split())
    ma = []
    for _ in range(N):
        list1 = list(input())
        ma.append(list1)

    for i in range(N):
        for j in range(M):
            w = 0
            if ma[i][j] == 'W':
                ans += 1
                ma[i][j] = '.'
                queue = [(i, j)]
                while queue:
                    x, y = queue.pop()
                    for dx in [-1, 0, 1]:
                        for dy in [-1, 0, 1]:
                            nx, ny = x + dx, y + dy
                            if 0 <= nx < N and 0 <= ny < M and ma[nx][ny] == 'W':
                                ma[nx][ny] = '.'
                                queue.append((nx, ny))
                                ans += 1
                            elif 0 <= nx < N and 0 <= ny < M and ma[nx][ny] == '.':
                                w += 1
            if ans > 0:
                re.append(ans)
                ans = 0

    re.append(ans)
    result.append(max(re))

for z in result:
    print(z)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240415211836231](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240415211836231.png)



### sy383: 最大权值连通块

https://sunnywhy.com/sfbj/10/3/383



思路：有了第一题手搓vertex和graph的经验，这题基本上稍微更改一下就可以了。

样例很善良，提醒了环状图的可能，所以用了一个列表al（表示already）来存储已经加过的权值。



代码

```python
# 
class Vertex:
    def __init__(self, value):
        self.value = value
        self.adjacent = []
        self.weight=None

class Graph:
    def __init__(self):
        self.ver = {}
        self.value=[]

    def add_edge(self, from_vert, to_vert):
        self.ver[from_vert].adjacent.append(to_vert)
        self.ver[to_vert].adjacent.append(from_vert)




n,m=map(int,input().split())
weights=list(map(int,input().split()))
g=Graph()
g.value=[i for i in range(n)]
output=[]
for i in range(n):
    g.ver[i]=Vertex(i)
    g.ver[i].weight=weights[i]


for i in range(m):
    u,v=map(int,input().split())
    g.add_edge(u,v)
al=[]
for i in g.value:
    ans=0
    ans+=g.ver[i].weight
    al.append(i)
    queue=[]
    queue.extend(g.ver[i].adjacent)
    g.ver[i].weight=0
    while queue:
        a=queue.pop()
        if a not in al:
            queue.extend(g.ver[a].adjacent)
            ans+=g.ver[a].weight
            g.ver[a].weight = 0
            al.append(a)
        else:
            continue
    output.append(ans)
print(max(output))




```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

*![image-20240415215058660](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240415215058660.png)**



### 03441: 4 Values whose Sum is 0

data structure/binary search, http://cs101.openjudge.cn/practice/03441



思路：自己一开始的思路是存两个字典，分别是AB和CD的所有组合，然后再进行遍历，发现会MLE，然后又试了只存AB字典，然后套双重循环对于C再遍历，在D中进行二分查找，发现TLE了，大概是因为在字典的设置中把元组设置成key，value设置为和，导致出现重复数对的时候修正逻辑的过程比较复杂，用到了列表的遍历，实际上套了三层n的for循环导致超时。所以参考了题解的写法，把字典的key设置为和，value设置为数对的出现次数，这样逻辑就顺畅多了，两层循环能够顺利AC，甚至不用二分查找都可以。



代码

```python
# 
n=int(input())
A=[]
B=[]
C=[]
D=[]
ans=0
for i in range(n):
    a,b,c,d=map(int,input().split())
    A.append(a)
    B.append(b)
    C.append(c)
    D.append(d)

#构建AB所有的元组
AB={}

for i in range(n):
    for j in range(n):
        if A[i]+B[j]  not in AB:
            AB[A[i]+B[j]]=1
        else:
            AB[A[i] + B[j]]+=1


for i in range(n):
    for j in range(n):
        if -(C[i]+D[j]) in AB:
            ans+=AB[-(C[i]+D[j])]

print(ans)

```



代码运行截图 

![image-20240416112030206](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240416112030206.png)



### 04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

Trie 数据结构可能需要自学下。



思路：找gpt自学了trie结构，大概就是把每个输入单词都转化为一棵树，每个数字都是前一个数字的子节点。如果从根节点顺着往下找能找到某个已知号码的全部数字，那么就说明前缀是一样的。第一次写的时候TLE了，因为没想清楚，对于每一个电话号码都建了一棵树，再把比它短的都遍历一遍，这样时间复杂度很高，后来发现可以从后往前遍历（即从长往短），对于每一个号码先判断是否是已知树的前缀，如果不是就把这个号码也加入到树中，这样只需要从头到尾扫一遍就可以了。



代码

```python
# 
class Node:
    def __init__(self):
        self.children={}


class Trie:
    def __init__(self):
        self.root=Node()

    def insert(self,word):
        node=self.root
        for char in word:
            if char not in node.children:
                node.children[char]=Node()
            node=node.children[char]

    def starts_with(self, given):
        node = self.root
        for char in given:
            if char in node.children:
                node = node.children[char]
            else:
                return False
        return True

t=int(input())

for i in range(t):
    tele = []
    n=int(input())
    for j in range(n):
        tele.append(input())
    tele = sorted(tele, key=lambda x: len(x),reverse=True)
    
    ans=0
    Tri = Trie()
    Tri.insert(tele[0])
    for x in range(1,n):
        if Tri.starts_with(tele[x]):
            print('NO')
            ans = 1
            break
        Tri.insert(tele[x])

    if ans==0:
        print('YES')
```



代码运行截图 

![image-20240416125329371](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240416125329371.png)



### 04082: 树的镜面映射

http://cs101.openjudge.cn/practice/04082/



思路：因为想了很久都没有思路，所以直接去参考了题解。先遍历树的右子节点，并将非虚节点加入栈 ，这样做是为了保证在print镜像映射序列时，从根节点开始从左到右输出，然后再将栈中的节点逆序放入队列 Q中，以便按照宽度优先的顺序print。

代码

```python
# 
from collections import deque

class TreeNode:
    def __init__(self, x):
        self.x = x
        self.children = []

def create_node():
    return TreeNode('')

def build_tree(tempList, index):
    node = create_node()
    node.x = tempList[index][0]
    if tempList[index][1] == '0' and node.x != '$':
        index += 1
        child, index = build_tree(tempList, index)
        node.children.append(child)
        index += 1
        child, index = build_tree(tempList, index)
        node.children.append(child)
    return node, index

def print_tree(p):
    Q = deque()
    s = deque()

    # 遍历右子节点并将非虚节点加入栈s
    while p is not None:
        if p.x != '$':
            s.append(p)
        p = p.children[1] if len(p.children) > 1 else None

    # 将栈s中的节点逆序放入队列Q
    while s:
        Q.append(s.pop())

    # 宽度优先遍历队列Q并打印节点值
    while Q:
        p = Q.popleft()
        print(p.x, end=' ')

        # 如果节点有左子节点，将左子节点及其右子节点加入栈s
        if p.children:
            p = p.children[0]
            while p is not None:
                if p.x != '$':
                    s.append(p)
                p = p.children[1] if len(p.children) > 1 else None

            # 将栈s中的节点逆序放入队列Q
            while s:
                Q.append(s.pop())

# 读取输入
n = int(input())
tempList = input().split(' ')

# 构建多叉树
root, _ = build_tree(tempList, 0)

# 执行宽度优先遍历并打印镜像映射序列
print_tree(root)
```



代码运行截图 

![image-20240416134651264](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240416134651264.png)



## 2. 学习总结和收获

这周的作业难度没有那么大，但是也揭示以前学习过的知识有很多需要复习的地方，比如说dfs和bfs的经典写法。手搓类是很有效的学习方法，graph和vertex自己搓出来之后基本上就把原理理解得差不多了，新学的Trie虽然是对着GPT一步一步走得，但是就算是照着写一遍也得明白每一步代码的逻辑和意义，所以感觉收获还是挺大的。最后一题的树看了题解才大概明白，可能还得再自己写一遍。


