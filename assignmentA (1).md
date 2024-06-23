# Assignment #A: 图论：算法，树算及栈

Updated 2018 GMT+8 Apr 21, 2024

2024 spring, Complied by 张惠雯，生命科学学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1



## 1. 题目

### 20743: 整人的提词本

http://cs101.openjudge.cn/practice/20743/



思路：运用了一个stack来进行存储和reverse。每次reverse完之后把元素重新insert到栈中，方便后续继续reverse

代码

```python
# 
a=input()
s=[]
for i in range(len (a)):
    s.append(a[i])

stack=[]
ans=[]
for i in range(len (s)):
    if s[i]==')':
        x=0
        while stack:
            a=stack.pop()
            x+=1
            if a=="(":
                break
            else:
                ans.append(a)
        ans.reverse()
        for j in range(x-1):
            stack.insert(i-x+j,ans.pop())



    else:
        stack.append(s[i])

print(''.join(stack))




```



代码运行截图 ==（至少包含有"Accepted"）==![image-20240430223408602](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240430223408602.png)



### 02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/



思路：参考了之前写的通过后序和中序建树的题目，用到了一个结论，即后序遍历=前序遍历先遍历右子树再进行一次reverse。



代码

```python
#
class Node:
    def __init__(self,val):
        self.left=None
        self.right=None
        self.value=val

def build_tree(midtree,pretree,output):#递归函数
        root=pretree[0]

        output.append(root)
        root_index=midtree.index(root)#将mid和post都切分为左子树和右子树
        mid_left_tree=midtree[0:root_index]
        mid_right_tree=midtree[root_index+1:len(midtree)]
        pre_left_tree=pretree[1:1+len(mid_left_tree)]
        pre_right_tree=pretree[1+len(mid_left_tree):len(pretree)]
        if pre_right_tree:
            build_tree(mid_right_tree,pre_right_tree,output)
        if pre_left_tree:#递归调用
            build_tree(mid_left_tree,pre_left_tree,output)


        return output




while True:
    try:
        a,b=input().split()
        pre,mid=[],[]
        for i in range(len(a)):
            pre.append(a[i])
            mid.append(b[i])





        ans=build_tree(mid,pre,[])
        ans.reverse()

        print(''.join(ans))
    except EOFError:
        break




```



代码运行截图 ==（至少包含有"Accepted"）==



![image-20240430215803975](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240430215803975.png)

### 01426: Find The Multiple

http://cs101.openjudge.cn/practice/01426/

要求用bfs实现

大概的思路是：使用一个deque来进行数字的寻找，同时开一个visited的集合来存储已经有的mod，防止反复寻找。（mod一样的数字的实际意义是等价的）

每次从deque的左侧popleft一个数字出来，对其进行尾数+1和+0的分别操作，并且计算新的mod（方法：原来的mod*10+尾数，再除以n），如果mod没有在visited里，才把这个新构建的数字入队。

代码

```python
from collections import deque


def find_multiple(n):
    q=deque()
    q.append('1')
    visited=set()
    visited.add(1%n)
    while q:
        a=int(q.popleft())
        if a%int(n)==0:
            return a
        else:
            num=['0','1']
            for i in range(2):
                mod=((a%int(n))*10+int(num[i]))%int(n)
                if mod not in visited:
                    visited.add(mod)
                    q.append(str(a)+num[i])

while True:
    n = int(input())
    if n!=0:
        print(find_multiple(n))

    elif n==0:
        break


```



代码运行截图 

![image-20240501220940017](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240501220940017.png)



### 04115: 鸣人和佐助

bfs, http://cs101.openjudge.cn/practice/04115/



思路：写了好一阵一直re，所以直接去参考了题解。大概的思路是使用bfs，通过一个队列queue来依次处理节点。从起点开始，判断四个方向的节点，是'*'表示可以直接通过，将其加入队列并标记为已访问；

是'#'表示需要使用工具才能通过，接下来判断当前节点是否还有工具可用，如果有则减少一个工具，并将该节点加入队列并标记为已访问。

如果队列为空，即无法到达终点，则输出"-1"；如果找到终点，输出步数，并将flag标记为1，退出循环。

易错的地方在于要把tool的数量作为一个参数一起传入visited，因为可能有不同的路径走到当前位置的时候的剩余tool数量不同，不然会导致一直输出-1.



代码

```python

# 
'''
这段代码是一个迷宫求解的问题。主要使用了广度优先搜索算法来找到从起点到终点的最短路径。

首先，代码导入了deque模块来实现队列数据结构。
然后，定义了一个Node类，表示迷宫中的一个节点。它有四个属性：x和y表示节点的坐标，
tools表示节点当前拥有的工具数，steps表示从起点到达该节点的步数。

接下来，读取输入的迷宫信息，包括迷宫的大小M和N，以及可以使用的工具数T。maze是一个二维列表，
表示迷宫的格子，其中'@'表示起点，'+'表示终点，'*'表示障碍物。

创建了一个visit列表，用于记录节点是否被访问过。visit是一个三维列表，
三个维度分别表示行、列和工具数。

定义了directions列表，包含四个方向的偏移量。

通过遍历迷宫，找到起点和终点的位置，并设置起点节点的属性。

使用广度优先搜索算法，通过一个队列queue来依次处理节点。从起点开始，判断四个方向的相邻节点：
如果是'*'表示可以直接通过，将其加入队列并标记为已访问；如果是'#'表示需要使用一个工具，
才能通过，判断当前节点是否还有工具可用，如果有则减少一个工具，并将该节点加入队列并标记为已访问。

如果队列为空，即无法到达终点，则输出"-1"；如果找到终点，输出步数，并将flag标记为1，退出循环。

最后，判断flag是否为0，如果是说明无法找到终点，输出"-1"。
'''

from collections import deque


class Node:
    def __init__(self, x, y, tools, steps):
        self.x = x
        self.y = y
        self.tools = tools
        self.steps = steps


M, N, T = map(int, input().split())
maze = [list(input()) for _ in range(M)]
visit = [[[0]*(T+1) for _ in range(N)] for _ in range(M)]
directions = [[-1, 0], [1, 0], [0, -1], [0, 1]]
start = end = None
flag = 0
for i in range(M):
    for j in range(N):
        if maze[i][j] == '@':
            start = Node(i, j, T, 0)
            visit[i][j][T] = 1
        if maze[i][j] == '+':
            end = (i, j)
            maze[i][j] = '*'
            
queue = deque([start])
while queue:
    node = queue.popleft()
    if (node.x, node.y) == end:
        print(node.steps)
        flag = 1
        break
    for direction in directions:
        nx, ny = node.x+direction[0], node.y+direction[1]
        if 0 <= nx < M and 0 <= ny < N:
            if maze[nx][ny] == '*':
                if not visit[nx][ny][node.tools]:
                    queue.append(Node(nx, ny, node.tools, node.steps+1))
                    visit[nx][ny][node.tools] = 1
            elif maze[nx][ny] == '#':
                if node.tools > 0 and not visit[nx][ny][node.tools-1]:
                    queue.append(Node(nx, ny, node.tools-1, node.steps+1))
                    visit[nx][ny][node.tools-1] = 1
                    
if not flag:
    print("-1")

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）=

![image-20240430224247313](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240430224247313.png)





### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/



思路：



代码

```python
def bfs(x, y):
    # 定义方向的偏移量
    directions = [(0, -1), (0, 1), (1, 0), (-1, 0)]
    # 初始化队列，将起点加入队列
    queue = [(x, y)]
    # 初始化距离字典，将起点的距离设为0，其他节点设为无穷大
    distances = {(x, y): 0}

    while queue:
        # 弹出队列中的节点
        current_x, current_y = queue.pop(0)

        # 遍历四个方向上的相邻节点
        for dx, dy in directions:
            new_x, new_y = current_x + dx, current_y + dy

            # 判断新节点是否在合法范围内
            if 0 <= new_x < m and 0 <= new_y < n:
                # 判断新节点是否为墙
                if d[new_x][new_y] != '#':
                    # 计算新节点的距离
                    new_distance = distances[(current_x, current_y)] + abs(int(d[new_x][new_y]) - int(d[current_x][current_y]))

                    # 如果新节点的距离小于已记录的距离，则更新距离字典和队列
                    if (new_x, new_y) not in distances or new_distance < distances[(new_x, new_y)]:
                        distances[(new_x, new_y)] = new_distance
                        queue.append((new_x, new_y))

    return distances

# 读取输入
m, n, p = map(int, input().split())
d = []
for _ in range(m):
    row = input().split()
    d.append(row)

for _ in range(p):
    # 读取起点和终点坐标
    x1, y1, x2, y2 = map(int, input().split())

    # 判断起点和终点是否为墙
    if d[x1][y1] == '#' or d[x2][y2] == '#':
        print('NO')
        continue

    # 使用BFS计算最短距离
    distances = bfs(x1, y1)

    # 输出结果，如果终点在距离字典中，则输出对应的最短距离，否则输出'NO'
    if (x2, y2) in distances:
        print(distances[(x2, y2)])
    else:
        print('NO')
```



代码运行截图 

![image-20240430224925286](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240430224925286.png)



### 05442: 兔子与星空

Prim, http://cs101.openjudge.cn/practice/05442/



思路：



代码

```python
# 
import heapq

def prim(graph, start):
    mst = []
    used = set([start])
    edges = [
        (cost, start, to)
        for to, cost in graph[start].items()
    ]
    heapq.heapify(edges)

    while edges:
        cost, frm, to = heapq.heappop(edges)
        if to not in used:
            used.add(to)
            mst.append((frm, to, cost))
            for to_next, cost2 in graph[to].items():
                if to_next not in used:
                    heapq.heappush(edges, (cost2, to, to_next))

    return mst

def solve():
    n = int(input())
    graph = {chr(i+65): {} for i in range(n)}
    for i in range(n-1):
        data = input().split()
        star = data[0]
        m = int(data[1])
        for j in range(m):
            to_star = data[2+j*2]
            cost = int(data[3+j*2])
            graph[star][to_star] = cost
            graph[to_star][star] = cost
    mst = prim(graph, 'A')
    print(sum(x[2] for x in mst))

solve()

```



代码运行截图 

![image-20240430225624000](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240430225624000.png)



## 2. 学习总结和收获

期中终于结束了，这周末花了比平时多很多的时间在数算上，但是发现bfs还是很不熟练，稍微复杂一些的题目都需要参考题解，上课讲的算法虽然都能理解，自己写的时候还是磕磕巴巴。打算后面几天假期突击一下，过一遍课件，再把涉及的代码都手搓一遍，顺便复习一下笔试知识点。



