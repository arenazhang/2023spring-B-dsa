# Assignment #9: 图论：遍历，及 树算

Updated 1739 GMT+8 Apr 14, 2024

2024 spring, Complied by 张惠雯，生命科学学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1



## 1. 题目

### 04081: 树的转换

http://cs101.openjudge.cn/dsapre/04081/



思路：一直都理解左孩子右兄弟的原理，但是自己上手写的时候还是样例能过但是WA了，请GPT帮忙修改了一下转换树部分的函数就AC了。具体一开始出现的问题在于更新left-child的逻辑出了问题，最后无法指向每层的最左节点。



代码

```python
# 
class Node:
    def __init__(self):
        self.children=[]
        self.parent=None
        self.left=None
        self.right=None

def build_tree(node):
    if node:
        x=Node()
        x.parent=node

        node.children.append(x)
        return x

def initial_tree_height(root,ans):
    global output1
    if root.children:
        for i in root.children:
            initial_tree_height(i,ans+1)
    else:
        output1.append(ans)

def binary_tree_height(node):
    if node is None:
        return -1
    left_height=binary_tree_height(node.left)
    right_height=binary_tree_height(node.right)
    return max(left_height,right_height)+1

def change_tree(root):
    if not root:
        return None

    if len(root.children) > 0:
        left_child = change_tree(root.children[0])
        root.left = left_child
        left_child.parent = root
        for i in range(1, len(root.children)):
            right_child = change_tree(root.children[i])
            left_child.right = right_child
            right_child.parent = left_child
            left_child = right_child
    return root





a=input()
#build initial tree
root=Node()
node=root
for i in range(len(a)):
    if a[i]=='d':
        node=build_tree(node)
    else:
        x=node
        node=x.parent

output1=[]
initial_tree_height(root,0)
initial_height=max(output1)#初始树高度
change_tree(root)
post_height=binary_tree_height(root)
print(initial_height,'=>',post_height)






```



代码运行截图 



![image-20240418121952900](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240418121952900.png)

### 08581: 扩展二叉树

http://cs101.openjudge.cn/dsapre/08581/



思路：首先通过已给的先序遍历建一个满二叉树，然后在中序和后序遍历的时候把其中的’.‘不加入列表即可。思路还是比较清晰的，主要是一段时间没有自己写树了，所以花了一些时间自己手搓满二叉树的建立过程，一遍AC.

代码

```python
# 
class Node:
    def __init__(self,value):
        self.left=None
        self.right=None
        self.val=value
        self.parent=None
a=input()
post_tree=[]
for i in a:
    post_tree.append(Node(i))

#后续遍历：

post=[]
def postorder(node):
    global post
    if node.left:
        postorder(node.left)
    if node.right:
        postorder(node.right)
    if node.val!='.':
        post.append(node.val)
#中序遍历
mid=[]
def midorder(node):
    global mid
    if node.left:
        midorder(node.left)
    if node.val!='.':
        mid.append(node.val)
    if node.right:
        midorder(node.right)

def build_post_tree(root,node):
    if root.val=='.' :
        return build_post_tree(root.parent,node)
    else:
        if root.left==None:
            root.left=node
            node.parent=root
            return node
        elif root.left and root.right==None:
            root.right=node
            node.parent=root
            return node
        elif root.left and root.right:
            return build_post_tree(root.parent,node)
root=post_tree[0]


for i in range(1,len(a)):
    root=build_post_tree(root,post_tree[i])

midorder(post_tree[0])
print(''.join(mid))

postorder(post_tree[0])
print(''.join(post))









```



代码运行截图 

![image-20240418165058238](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240418165058238.png)





### 22067: 快速堆猪

http://cs101.openjudge.cn/practice/22067/



思路：一开始就想到可以用heap，但是发现pop的时候并不是直接pop最小的，而引入了堆之后如果再remove一个特定的元素也很麻烦（TLE）,所以去学习了题解的辅助栈做法。根本思路是辅助栈中存储当前栈中的最小值。每次执行 push 操作时，如果新元素比辅助栈中的栈顶元素更小，则将新元素也压入辅助栈；否则，将辅助栈栈顶元素再次压入辅助栈。这样，辅助栈的栈顶元素始终是当前栈中的最小值。

特别地，一开始我没有理解为什么这段代码在pop的时候不用检查是否pop出的元素就是最小值，后来发现逻辑如下：

如果pop出的是最小值，说明最小值刚被压入栈中，此时辅助栈的栈顶就是最小值，第二个元素是第二小的值，因此即使pop出来最小值之后通过代码逻辑也可以保证辅助栈栈顶还是此时栈中的最小元素，非常巧妙，原因是在每次push的时候都相应push一个元素进辅助栈，使得辅助栈与标准栈始终保持一一对应的关系。



代码

```python
# 
pig1=[]
pig2=[]
while True:
    try:
        a=input().split()
        if a[0]=='pop':
            if pig1:
                pig1.pop()
                if pig2:
                    pig2.pop()

        elif a[0]=='min':
            if pig2:
                print(pig2[-1])

        else:
            b=int(a[1])
            pig1.append(b)
            if not pig2:
                pig2.append(b)
            else:
                minn=min(pig2[-1],b)
                pig2.append(minn)
    except EOFError:
        break

```



代码运行截图 

![image-20240418224608869](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240418224608869.png)





### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123



思路：因为自己写dfs还是不熟练，与其自己死磕写出来一堆又长又多bug的代码不如直接学习题解。

发现自己最容易出错的地方就是在bfs的回溯更新部分。



代码

```python
# 
maxn = 10;
sx = [-2,-1,1,2, 2, 1,-1,-2]
sy = [ 1, 2,2,1,-1,-2,-2,-1]

ans = 0;
 
def Dfs(dep: int, x: int, y: int):
    #是否已经全部走完
    if n*m == dep:
        global ans
        ans += 1
        return
    
    #对于每个可以走的点
    for r in range(8):
        s = x + sx[r]
        t = y + sy[r]
        if chess[s][t]==False and 0<=s<n and 0<=t<m :
            chess[s][t]=True
            Dfs(dep+1, s, t)
            chess[s][t] = False; #回溯
 

for _ in range(int(input())):
    n,m,x,y = map(int, input().split())
    chess = [[False]*maxn for _ in range(maxn)]  #False表示没有走过
    ans = 0
    chess[x][y] = True
    Dfs(1, x, y)
    print(ans)
```



代码运行截图 

![image-20240422142439129](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240422142439129.png)





### 28046: 词梯

bfs, http://cs101.openjudge.cn/practice/28046/



思路：虽然上周自己写了几次bfs，但是在自己写词梯的时候还是遇到了逻辑是线上的困难，debug一个小时之后去看了题解，发现需要用到之前没有自己实现过的桶结构。

基本看懂了题解的思路，感觉又学到了新东西。



代码

```python
# 
import sys
from collections import deque

class Graph:
    def __init__(self):
        self.vertices = {}
        self.num_vertices = 0

    def add_vertex(self, key):
        self.num_vertices = self.num_vertices + 1
        new_vertex = Vertex(key)
        self.vertices[key] = new_vertex
        return new_vertex

    def get_vertex(self, n):
        if n in self.vertices:
            return self.vertices[n]
        else:
            return None

    def __len__(self):
        return self.num_vertices

    def __contains__(self, n):
        return n in self.vertices

    def add_edge(self, f, t, cost=0):
        if f not in self.vertices:
            nv = self.add_vertex(f)
        if t not in self.vertices:
            nv = self.add_vertex(t)
        self.vertices[f].add_neighbor(self.vertices[t], cost)

    def get_vertices(self):
        return list(self.vertices.keys())

    def __iter__(self):
        return iter(self.vertices.values())


class Vertex:
    def __init__(self, num):
        self.key = num
        self.connectedTo = {}
        self.color = 'white'
        self.distance = sys.maxsize
        self.previous = None
        self.disc = 0
        self.fin = 0

    def add_neighbor(self, nbr, weight=0):
        self.connectedTo[nbr] = weight

    # def setDiscovery(self, dtime):
    #     self.disc = dtime
    #
    # def setFinish(self, ftime):
    #     self.fin = ftime
    #
    # def getFinish(self):
    #     return self.fin
    #
    # def getDiscovery(self):
    #     return self.disc

    def get_neighbors(self):
        return self.connectedTo.keys()

    # def getWeight(self, nbr):
    #     return self.connectedTo[nbr]

    # def __str__(self):
    #     return str(self.key) + ":color " + self.color + ":disc " + str(self.disc) + ":fin " + str(
    #         self.fin) + ":dist " + str(self.distance) + ":pred \n\t[" + str(self.previous) + "]\n"



def build_graph(all_words):
    buckets = {}
    the_graph = Graph()

    # 创建词桶 create buckets of words that differ by 1 letter
    for line in all_words:
        word = line.strip()
        for i, _ in enumerate(word):
            bucket = f"{word[:i]}_{word[i + 1:]}"
            buckets.setdefault(bucket, set()).add(word)

    # 为同一个桶中的单词添加顶点和边
    for similar_words in buckets.values():
        for word1 in similar_words:
            for word2 in similar_words - {word1}:
                the_graph.add_edge(word1, word2)

    return the_graph


def bfs(start, end):
    start.distnce = 0
    start.previous = None
    vert_queue = deque()
    vert_queue.append(start)
    while len(vert_queue) > 0:
        current = vert_queue.popleft()  # 取队首作为当前顶点

        if current == end:
            return True

        for neighbor in current.get_neighbors():  # 遍历当前顶点的邻接顶点
            if neighbor.color == "white":
                neighbor.color = "gray"
                neighbor.distance = current.distance + 1
                neighbor.previous = current
                vert_queue.append(neighbor)
        current.color = "black"  # 当前顶点已经处理完毕，设黑色

    return False

"""
BFS 算法主体是两个循环的嵌套: while-for
    while 循环对图中每个顶点访问一次，所以是 O(|V|)；
    嵌套在 while 中的 for，由于每条边只有在其起始顶点u出队的时候才会被检查一次，
    而每个顶点最多出队1次，所以边最多被检查次，一共是 O(|E|)；
    综合起来 BFS 的时间复杂度为 0(V+|E|)

词梯问题还包括两个部分算法
    建立 BFS 树之后，回溯顶点到起始顶点的过程，最多为 O(|V|)
    创建单词关系图也需要时间，时间是 O(|V|+|E|) 的，因为每个顶点和边都只被处理一次
"""


def traverse(starting_vertex):
    ans = []
    current = starting_vertex
    while (current.previous):
        ans.append(current.key)
        current = current.previous
    ans.append(current.key)

    return ans


n = int(input())
all_words = []
for _ in range(n):
    all_words.append(input().strip())

g = build_graph(all_words)
# print(len(g))

s, e = input().split()
start, end = g.get_vertex(s), g.get_vertex(e)
if start is None or end is None:
    print('NO')
    exit(0)

if bfs(start, end):
    ans = traverse(end)
    print(' '.join(ans[::-1]))
else:
    print('NO')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/



思路：自己写的时候一直WA，所以直接看了题解。基本的实现思路和马走日是一个道理，但是需要优化。

优化的具体方法是先找到当前点能够走的所有下一个点，然后计算每个下一个点能走的下下一个点数量，优先搜索数量少的，一旦发现一条周游路径就返回True。



代码

```python
# 
import sys

class Graph:
    def __init__(self):
        self.vertices = {}
        self.num_vertices = 0

    def add_vertex(self, key):
        self.num_vertices = self.num_vertices + 1
        new_ertex = Vertex(key)
        self.vertices[key] = new_ertex
        return new_ertex

    def get_vertex(self, n):
        if n in self.vertices:
            return self.vertices[n]
        else:
            return None

    def __len__(self):
        return self.num_vertices

    def __contains__(self, n):
        return n in self.vertices

    def add_edge(self, f, t, cost=0):
        if f not in self.vertices:
            nv = self.add_vertex(f)
        if t not in self.vertices:
            nv = self.add_vertex(t)
        self.vertices[f].add_neighbor(self.vertices[t], cost)
        #self.vertices[t].add_neighbor(self.vertices[f], cost)

    def getVertices(self):
        return list(self.vertices.keys())

    def __iter__(self):
        return iter(self.vertices.values())


class Vertex:
    def __init__(self, num):
        self.key = num
        self.connectedTo = {}
        self.color = 'white'
        self.distance = sys.maxsize
        self.previous = None
        self.disc = 0
        self.fin = 0

    def __lt__(self,o):
        return self.key < o.key

    def add_neighbor(self, nbr, weight=0):
        self.connectedTo[nbr] = weight


    # def setDiscovery(self, dtime):
    #     self.disc = dtime
    #
    # def setFinish(self, ftime):
    #     self.fin = ftime
    #
    # def getFinish(self):
    #     return self.fin
    #
    # def getDiscovery(self):
    #     return self.disc

    def get_neighbors(self):
        return self.connectedTo.keys()

    # def getWeight(self, nbr):
    #     return self.connectedTo[nbr]

    def __str__(self):
        return str(self.key) + ":color " + self.color + ":disc " + str(self.disc) + ":fin " + str(
            self.fin) + ":dist " + str(self.distance) + ":pred \n\t[" + str(self.previous) + "]\n"



def knight_graph(board_size):
    kt_graph = Graph()
    for row in range(board_size):           #遍历每一行
        for col in range(board_size):       #遍历行上的每一个格子
            node_id = pos_to_node_id(row, col, board_size) #把行、列号转为格子ID
            new_positions = gen_legal_moves(row, col, board_size) #按照 马走日，返回下一步可能位置
            for row2, col2 in new_positions:
                other_node_id = pos_to_node_id(row2, col2, board_size) #下一步的格子ID
                kt_graph.add_edge(node_id, other_node_id) #在骑士周游图中为两个格子加一条边
    return kt_graph

def pos_to_node_id(x, y, bdSize):
    return x * bdSize + y

def gen_legal_moves(row, col, board_size):
    new_moves = []
    move_offsets = [                        # 马走日的8种走法
        (-1, -2),  # left-down-down
        (-1, 2),  # left-up-up
        (-2, -1),  # left-left-down
        (-2, 1),  # left-left-up
        (1, -2),  # right-down-down
        (1, 2),  # right-up-up
        (2, -1),  # right-right-down
        (2, 1),  # right-right-up
    ]
    for r_off, c_off in move_offsets:
        if (                                # #检查，不能走出棋盘
            0 <= row + r_off < board_size
            and 0 <= col + c_off < board_size
        ):
            new_moves.append((row + r_off, col + c_off))
    return new_moves

# def legal_coord(row, col, board_size):
#     return 0 <= row < board_size and 0 <= col < board_size


def knight_tour(n, path, u, limit):
    u.color = "gray"
    path.append(u)              #当前顶点涂色并加入路径
    if n < limit:
        neighbors = ordered_by_avail(u) #对所有的合法移动依次深入
        #neighbors = sorted(list(u.get_neighbors()))
        i = 0

        for nbr in neighbors:
            if nbr.color == "white" and \
                knight_tour(n + 1, path, nbr, limit):   #选择“白色”未经深入的点，层次加一，递归深入
                return True
        else:                       #所有的“下一步”都试了走不通
            path.pop()              #回溯，从路径中删除当前顶点
            u.color = "white"       #当前顶点改回白色
            return False
    else:
        return True

def ordered_by_avail(n):
    res_list = []
    for v in n.get_neighbors():
        if v.color == "white":
            c = 0
            for w in v.get_neighbors():
                if w.color == "white":
                    c += 1
            res_list.append((c,v))
    res_list.sort(key = lambda x: x[0])
    return [y[1] for y in res_list]

# class DFSGraph(Graph):
#     def __init__(self):
#         super().__init__()
#         self.time = 0                   #不是物理世界，而是算法执行步数
# 
#     def dfs(self):
#         for vertex in self:
#             vertex.color = "white"      #颜色初始化
#             vertex.previous = -1
#         for vertex in self:             #从每个顶点开始遍历
#             if vertex.color == "white":
#                 self.dfs_visit(vertex)  #第一次运行后还有未包括的顶点
#                                         # 则建立森林
# 
#     def dfs_visit(self, start_vertex):
#         start_vertex.color = "gray"
#         self.time = self.time + 1       #记录算法的步骤
#         start_vertex.discovery_time = self.time
#         for next_vertex in start_vertex.get_neighbors():
#             if next_vertex.color == "white":
#                 next_vertex.previous = start_vertex
#                 self.dfs_visit(next_vertex)     #深度优先递归访问
#         start_vertex.color = "black"
#         self.time = self.time + 1
#         start_vertex.closing_time = self.time


def main():
    def NodeToPos(id):
       return ((id//8, id%8))

    bdSize = int(input())  # 棋盘大小
    *start_pos, = map(int, input().split())  # 起始位置
    g = knight_graph(bdSize)
    start_vertex = g.get_vertex(pos_to_node_id(start_pos[0], start_pos[1], bdSize))
    if start_vertex is None:
        print("fail")
        exit(0)

    tour_path = []
    done = knight_tour(0, tour_path, start_vertex, bdSize * bdSize-1)
    if done:
        print("success")
    else:
        print("fail")

    exit(0)

    # 打印路径
    cnt = 0
    for vertex in tour_path:
        cnt += 1
        if cnt % bdSize == 0:
            print()
        else:
            print(vertex.key, end=" ")
            #print(NodeToPos(vertex.key), end=" ")   # 打印坐标

if __name__ == '__main__':
    main()
```



代码运行截图 

![image-20240422143344197](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240422143344197.png)





## 2. 学习总结和收获

因为这周有多门期中考和ddl，所以放在数算上的时间就相对少了一些，前面三题都没花太多时间就自己独立写出来了，但是到后面的bfs和dfs部分还是很不熟练，最后都只能去学习题解。题解的思路是都明白了，但是如果要自己独立再写出来可能还是有难度。等到这周期中结束会多花一些时间再把这次作业复盘一下，争取把参考了题解的题目都自己独立写一遍。





