# Assignment #B: 图论和树算

Updated 1709 GMT+8 Apr 28, 2024

2024 spring, Complied by 生命科学学院，张惠雯



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1



## 1. 题目

### 28170: 算鹰

dfs, http://cs101.openjudge.cn/practice/28170/



思路：一开始被题目忽悠了好久，还纳闷为什么找十字要用dfs，一直WA，然后看了群里的讨论才发现是找连通块的个数...跟上学期最基本的dfs套路一样，看懂题目之后5minAC了。



代码

```python
# 
ans=0
visited=[[False] * 10 for _ in range(10)]
def find_eagle(x,y,board):
    global visited
    global ans
    visited[x][y]=True
    k=0

    if x-1>=0:
        if board[x-1][y]=='.':
                k+=1
                if visited[x-1][y]==False:
                    find_eagle(x-1,y,board)

    if x+1<10:
        if board[x+1][y]=='.':
                k+=1
                if visited[x + 1][y] == False:
                    find_eagle(x+1,y,board)


    if y-1>=0:
        if board[x][y-1]=='.':
                k+=1
                if visited[x][y-1] == False:
                    find_eagle(x,y-1,board)

    if y+1<10:
        if board[x][y+1]=='.':
                k+=1
                if visited[x][y+1] == False:
                    find_eagle(x,y+1,board)


board=[]
for i in range(10):
    x=input()
    y=[]
    for n in x:
        y.append(n)
    board.append(y)



for i in range(10):
    for j in range(10):
        if visited[i][j]==False and board[i][j]=='.':
            find_eagle(i,j,board)
            ans+=1

print(ans)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240505205351285](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240505205351285.png)



### 02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/



思路：有了前几次的dfs练习，这次八皇后也成功自己独立写了出来，比上学期计概b只能看懂题解有所进步。大概的思路就是按照行数进行遍历，用三个visited数组来判断列和斜边是否已经有皇后。当x=7的时候走到了最后一行，就把对应的皇后串进行处理，加入到solution中。最后按照题意找出对应位置的皇后串输出即可。



代码

```python
# 
board=[[0]*8 for i in range(8)]
solutions=[]
visited_y=set()#存储有皇后的列
visited_sum=set()
visited_dif=set()
def dfs(x,y,board,visited_y,visited_sum):
    global solutions




    if y not in visited_y and x+y not in visited_sum and x-y not in visited_dif:
        board[x][y]=1
        visited_y.add(y)
        visited_sum.add(x+y)
        visited_dif.add(x-y)

        if x == 7:
            output = []
            for i in range(8):
                for j in range(8):
                    if board[i][j] == 1:
                        output.append(str(j + 1))
            ans = int(''.join(output))
            solutions.append(ans)
                  
        if x<7:
            for i in range(8):
                dfs(x+1,i,board,visited_y,visited_sum)

        board[x][y] = 0
        visited_y.discard(y)
        visited_sum.discard(x + y)
        visited_dif.discard(x - y)









for i in range(8):
    dfs(0,i,board,visited_y,visited_sum)

solutions.sort()
n=int(input())

for i in range(n):
    a=int(input())
    print(solutions[a-1])
```



代码运行截图 

![image-20240506103956344](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240506103956344.png)



### 03151: Pots

bfs, http://cs101.openjudge.cn/practice/03151/



思路：一开始有大概的想法，看出来是一个一维的bfs，跟之前对数进行操作达到规定数的题目思路很像，但是这题稍微复杂一点。问了GPT一下大概的思路，然后写了一个类，参数包括两个桶当前分别的水量，这步操作的上一步操作（方便最后的回溯）以及该操作的名称。个人感觉这个思路很简单，写起来逻辑也很顺滑，就是稍微有点长容易写错变量名。

一开始MLE了，用了visited数组才AC。



代码

```python
# 
class Operation:
    def __init__(self,v):
        self.x=None
        self.y=None
        self.last_op=None
        self.value=v



from collections import deque
a,b,c=map(int,input().split())
q=deque()
O1=Operation('FILL(1)')
O1.x=a
O1.y=0
O2=Operation('FILL(2)')
O2.y=b
O2.x=0
q.append(O1)
q.append(O2)
k=0
visited=set()
visited.add((a,0))
visited.add((0,b))

while q:

    global O
    O=q.popleft()
    if O.x!=c and O.y!=c:




            O1=Operation('FILL(1)')
            O1.x=a
            O1.last_op=O
            O1.y=O.y
            if (O1.x,O1.y) not in visited:
                q.append(O1)
                visited.add((O1.x,O1.y))

            O2=Operation('DROP(1)')
            O2.x = 0
            O2.last_op = O
            O2.y = O.y
            if (O2.x,O2.y) not in visited:
                q.append(O2)
                visited.add((O2.x, O2.y))


            O3 = Operation("POUR(1,2)")
            O3.x = max(0,O.x-(b-O.y))
            O3.last_op = O
            O3.y = min(b,O.y+O.x)
            if (O3.x, O3.y) not in visited:
                q.append(O3)
                visited.add((O3.x, O3.y))


            O4=Operation('FILL(2)')
            O4.y = b
            O4.last_op = O
            O4.x = O.x
            if (O4.x, O4.y) not in visited:
                q.append(O4)
                visited.add((O4.x, O4.y))

            O5 = Operation('DROP(2)')
            O5.y = 0
            O5.last_op = O
            O5.x = O.x
            if (O5.x,O5.y) not in visited:
                q.append(O5)
                visited.add((O5.x, O5.y))


            O6 = Operation("POUR(2,1)")
            O6.y = max(0, O.y - (a - O.x))
            O6.last_op = O
            O6.x = min(a, O.x + O.y)
            if (O6.x, O6.y) not in visited:
                q.append(O6)
                visited.add((O6.x, O6.y))

    elif O.x==c or O.y==c:
        k=1
        break

    elif O.x >c+min(a,b) or O.y>c+min(a,b):
        break
if k==1:
    ans=1
    output=[]
    while O.last_op:
        output.append(O.value)
        O=O.last_op
        ans+=1
    output.append(O.value)
    print(ans)
    output.reverse()
    for i in range(ans):
        print(output[i])
else:
    print('impossible')






```



代码运行截图 

![image-20240506005923536](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240506005923536.png)





### 05907: 二叉树的操作

http://cs101.openjudge.cn/practice/05907/



思路：首先构建一个列表，存储所有的树节点，因为节点的值是从0~n-1，所以初始建树很方便，把列表的下标和节点的value一一对应即可，这样后续操作的时候只需要通过下标即可访问节点。比较容易搞混的是交换节点的过程，注意除了更新parent，还要判断原来是左节点还是右节点，把更换后的新节点接上去，所以在node类里加了一个参数di，用来标记该节点是左节点还是右节点。



代码

```python
# 
class Node:
    def __init__(self,val):
        self.left=None
        self.right=None
        self.value=val
        self.parent=None
        self.di=None


t=int(input())
for i in range(t):
    n,m=map(int,input().split())
    nodes=[]
    for i in range(n):
        nodes.append(Node(i))
    for i in range(n):
        x,y,z=map(int,input().split())
        if y!=-1:
            nodes[x].left=nodes[y]
            nodes[y].parent=nodes[x]
            nodes[y].di='left'
        if z!=-1:
            nodes[x].right=nodes[z]
            nodes[z].parent=nodes[x]
            nodes[z].di='right'
    for i in range(m):
        a=list(map(int,input().split()))
        if a[0]==1:
            b=a[1]
            c=a[2]
            parent_b=nodes[b].parent
            parent_c=nodes[c].parent
            if nodes[c].di=='left' and nodes[b].di=='left':

                    nodes[c].parent=parent_b
                    parent_b.left=nodes[c]

                    nodes[b].parent=parent_c
                    parent_c.left=nodes[b]

            elif nodes[c].di=='right' and nodes[b].di=='right':

                    nodes[c].parent=parent_b
                    parent_b.right=nodes[c]

                    nodes[b].parent=parent_c
                    parent_c.right=nodes[b]

            elif nodes[c].di == 'left' and nodes[b].di == 'right':

                    nodes[c].parent=parent_b
                    parent_b.right=nodes[c]
                    nodes[c].di='right'

                    nodes[b].parent=parent_c
                    parent_c.left=nodes[b]
                    nodes[b].di='left'

            elif nodes[c].di == 'right' and nodes[b].di == 'left':

                nodes[c].parent = parent_b
                parent_b.left = nodes[c]
                nodes[c].di = 'left'

                nodes[b].parent = parent_c
                parent_c.right = nodes[b]
                nodes[b].di = 'right'
        else:
            left_child=nodes[a[1]]
            while left_child.left:
                left_child=left_child.left
            print(left_child.value)


```



代码运行截图 ==

![image-20240505200209527](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240505200209527.png)





### 18250: 冰阔落 I

Disjoint set, http://cs101.openjudge.cn/practice/18250/



思路：一开始按照自己的想法写了一个类，本来以为可以用没那么经典的并查集AC，因为觉得自己的逻辑也没有问题，结果还是栽在经典的问题上了，看了一眼群里的讨论错误的基本都是在更新根节点的时候没有同时更新所有其他在同一个cups里的可乐，导致出现逻辑错误。让GPT在我的代码基础上修改了一下就AC了，这些经典算法有的真的不能随便增减东西。



代码

```python
# 
class Node:
    def __init__(self, v):
        self.ancestor = self
        self.value = v
        self.children = []

def find_root(node):
    if node.ancestor != node:
        node.ancestor = find_root(node.ancestor) 
    return node.ancestor

while True:
    try:
        n, m = map(int, input().split())
        cups = [Node(i) for i in range(1, n + 1)]
        roots = set(range(1, n + 1))  # 初始时所有杯子都是根节点

        for i in range(m):
            x, y = map(int, input().split())
            root_x = find_root(cups[x - 1])
            root_y = find_root(cups[y - 1])
            if root_x == root_y:
                print('Yes')
            else:
                print('No')
                root_y.ancestor = root_x  # 合并
                roots.remove(root_y.value) 

        print(len(roots))  # 输出数量
        print(" ".join(map(str, sorted(roots))))  # 编号

    except EOFError:
        break

```



代码运行截图 

![image-20240506133232509](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240506133232509.png)





### 05443: 兔子与樱花

http://cs101.openjudge.cn/practice/05443/



思路：因为对dijkstra的掌握还不够，所以自己WA之后直接去参考了题解，然后又自己对着题解写了一遍，大概把实现思路理清楚了。

代码

```python
# 
import heapq

def dijkstra(adjacency, start):
    distances = {vertex: float('infinity') for vertex in adjacency}
    previous = {vertex: None for vertex in adjacency}
    distances[start] = 0
    pq = [(0, start)]

    while pq:
        current_distance, current_vertex = heapq.heappop(pq)
        if current_distance > distances[current_vertex]:
            continue

        for neighbor, weight in adjacency[current_vertex].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                previous[neighbor] = current_vertex
                heapq.heappush(pq, (distance, neighbor))

    return distances, previous

def shortest_path_to(adjacency, start, end):
    distances, previous = dijkstra(adjacency, start)
    path = []
    current = end
    while previous[current] is not None:
        path.insert(0, current)
        current = previous[current]
    path.insert(0, start)
    return path, distances[end]

# Read the input data
P = int(input())
places = {input().strip() for _ in range(P)}

Q = int(input())
graph = {place: {} for place in places}
for _ in range(Q):
    src, dest, dist = input().split()
    dist = int(dist)
    graph[src][dest] = dist
    graph[dest][src] = dist  # Assuming the graph is bidirectional

R = int(input())
requests = [input().split() for _ in range(R)]

# Process each request
for start, end in requests:
    if start == end:
        print(start)
        continue

    path, total_dist = shortest_path_to(graph, start, end)
    output = ""
    for i in range(len(path) - 1):
        output += f"{path[i]}->({graph[path[i]][path[i+1]]})->"
    output += f"{end}"
    print(output)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



![image-20240507002846967](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240507002846967.png)

## 2. 学习总结和收获

这周花了大概期中前两倍的时间在数算上，成就就是六道题有五题都自己独立做出来了没有参考题解，虽然有时候还是会有一些小bug要靠GPT修改，但是大的思路和实现方式都是没问题的。找回了一点写dfs和bfs的手感，特别是上学期让我一度无法理解的八皇后，一开始抱着试一试的想法动手开始写，结果思路越写越清晰，半小时顺利AC，还是挺惊喜的，说明确实能力有在进步。

最后一题还是遇到了一些问题，但是也基本学会了题解，接下来继续努力！投入的时间看到了回报！

