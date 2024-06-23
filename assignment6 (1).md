# Assignment #6: "树"算：Huffman,BinHeap,BST,AVL,DisjointSet

Updated 2214 GMT+8 March 24, 2024

2024 spring, Complied by 张惠雯，生命科学学院



**说明：**

1）这次作业内容不简单，耗时长的话直接参考题解。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1



## 1. 题目

### 22275: 二叉搜索树的遍历

http://cs101.openjudge.cn/practice/22275/



思路：自己写的时候样例能过但是一直WA，debug失败之后参考了题解1的写法，大致思路是递归，不断把树分成左子树和右子树，然后再递归处理。注意在更新根的左右节点的时候不能直接用root.left=pre[i]的写法，因为无法更新到后续pre[i]的子节点，而应该使用递归调用之后再返回已经更新了子节点的子节点，即从下往上地更新子节点。



代码

```python
#
class Tree:
    def __init__(self,v):
        self.left=None
        self.right=None
        self.value=v




def build_tree(pre):
    if not pre:
        return None

    root = pre[0]
    left_tree = []
    right_tree = []

    for i in range(1, len(pre)):
        if pre[i].value < root.value:
            left_tree.append(pre[i])
        else:
            right_tree = pre[i:]
            break

    root.left = build_tree(left_tree)
    root.right = build_tree(right_tree)

    return root




def output(node,ans):
    if node.left:
        output(node.left,ans)
    if node.right:
        output(node.right,ans)
    ans.append(node.value)
    return  ans










n=int(input())
if n==0:
    print(None)
else:
    pre_tree=list(map(int,input().split()))
    for i in range(len(pre_tree)):
        pre_tree[i]=Tree(pre_tree[i])

    if len(pre_tree)>1:
        build_tree(pre_tree)
        ans=output(pre_tree[0],[])
        for i in range(len(ans)):
            ans[i]=str(ans[i])
        print(' '.join(ans))
    elif len(pre_tree)==1:
        print(pre_tree[0].value)






```



代码运行截图



![image-20240401203553790](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240401203553790.png)

### 05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/



思路：基本上跟题解的思路是一致的，只是在数据的接收上有一些差别，比如题解中的去掉重复数字的写法就更加简洁（已积累到cheatsheet内），另一个一开始WA的原因是原来是写的pop（）即从后往前建树，输出的就完全不一样，后来观察了一下样例才意识到只能从前往后建树，以后还是得把样例彻底看懂了再开始写代码啊。



代码

```python
# 
class Tree:
    def __init__(self,v):
        self.right=None
        self.left=None
        self.value=v


def insert(root,node):
    if not root:
        return node

    if node.value>root.value:
        root.right=insert(root.right,node)

    elif node.value<root.value:
        root.left=insert(root.left,node)

    return root

def level_traversal(root):
    queue=[root]
    output=[]
    while queue:
        node=queue.pop(0)
        output.append(node.value)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return output

n=list(map(int,input().split()))
tree=[]
for i in n:
    if i not in tree:
        tree.append(i)
root=Tree(tree.pop(0))

while tree:
    node=tree.pop(0)
    node=Tree(node)
    root=insert(root,node)
output=level_traversal(root)
print(' '.join(map(str,output)))

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240401214842654](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240401214842654.png)



### 04078: 实现堆结构

http://cs101.openjudge.cn/practice/04078/

练习自己写个BinHeap。当然机考时候，如果遇到这样题目，直接import heapq。手搓栈、队列、堆、AVL等，考试前需要搓个遍。



思路：不知道为什么自己写的一直WA也找不出问题，干脆就照着题解的思路写了一遍，也算是掌握了写法。感觉在处理边界条件的时候需要思路特别清楚，否则在递归的过程中很容易index越界。



代码

```python
# 
class bin_heap:
    def __init__(self):
        self.heap_list=[0]
        self.current_size=0

    def per_up(self, i):
        while i // 2 > 0:
            if self.heap_list[i] < self.heap_list[i // 2]:
                tmp = self.heap_list[i]
                self.heap_list[i] = self.heap_list[i // 2]
                self.heap_list[i // 2] = tmp
                self.per_up(i // 2)
            else:
                break

    def per_down(self, i):
        while i * 2 <= self.current_size:
            mc = self.min_child(i)
            if self.heap_list[i] > self.heap_list[mc]:
                tmp = self.heap_list[i]
                self.heap_list[i] = self.heap_list[mc]
                self.heap_list[mc] = tmp
                self.per_down(mc)
            else:
                break

    def min_child(self, i):
        if i * 2 + 1 > self.current_size:
            return i * 2
        else:
            if self.heap_list[i * 2] < self.heap_list[i * 2 + 1]:
                return i * 2
            else:
                return i * 2 + 1





def insert_(heap,a):
    heap.heap_list.append(a)
    heap.current_size += 1
    heap.per_up(heap.current_size)



def del_min(heap):
    print(heap.heap_list.pop(1))
    heap.current_size-=1
    heap.heap_list.insert(1,heap.heap_list.pop())
    if len(heap.heap_list)>2:
        ob = heap.heap_list[1]
        heap.per_down(1)





n=int(input())
heap=bin_heap()
for i in range(n):
    a=input()
    if a[0]=='1':
        b=a.split()
        u=int(b[-1])
        insert_(heap,u)
    
    else:
        del_min(heap)
        











```



代码运行截图



![image-20240402113131088](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240402113131088.png)

### 22161: 哈夫曼编码树

http://cs101.openjudge.cn/practice/22161/



思路：基本思路和题解是一样的，但是自己写的时候一直RE，debug接近两个小时之后果断决定还是看题解。



代码

```python
# 
import heapq

class Node:
    def __init__(self, weight, char=None):
        self.weight = weight
        self.char = char
        self.left = None
        self.right = None

    def __lt__(self, other):
        if self.weight == other.weight:
            return self.char < other.char
        return self.weight < other.weight

def build_huffman_tree(characters):
    heap = []
    for char, weight in characters.items():
        heapq.heappush(heap, Node(weight, char))

    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        merged = Node(left.weight + right.weight)
        merged.left = left
        merged.right = right
        heapq.heappush(heap, merged)

    return heap[0]

def encode_huffman_tree(root):
    codes = {}

    def traverse(node, code):
        if node.char:
            codes[node.char] = code
        else:
            traverse(node.left, code + '0')
            traverse(node.right, code + '1')

    traverse(root, '')
    return codes

def huffman_encoding(codes, string):
    encoded = ''
    for char in string:
        encoded += codes[char]
    return encoded

def huffman_decoding(root, encoded_string):
    decoded = ''
    node = root
    for bit in encoded_string:
        if bit == '0':
            node = node.left
        else:
            node = node.right

        if node.char:
            decoded += node.char
            node = root
    return decoded

n = int(input())
characters = {}
for _ in range(n):
    char, weight = input().split()
    characters[char] = int(weight)


huffman_tree = build_huffman_tree(characters)


codes = encode_huffman_tree(huffman_tree)

strings = []
while True:
    try:
        line = input()
        if line:
            strings.append(line)
        else:
            break
    except EOFError:
        break

results = []

for string in strings:
    if string[0] in ('0','1'):
        results.append(huffman_decoding(huffman_tree, string))
    else:
        results.append(huffman_encoding(codes, string))

for result in results:
    print(result)
```



代码运行截图

![image-20240402141250574](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240402141250574.png)



### 晴问9.5: 平衡二叉树的建立

https://sunnywhy.com/sfbj/9/5/359



思路：上课的时候听AVL树的实现感觉思路和逻辑都特别清晰，判断不同的树形然后进行对应的旋转即可，相应的旋转方式也都是已知的，但是自己动手写的时候很无从下手，奋斗2h之后还是看了题解）主要就是定义了左旋右旋的函数，然后在insert的函数内部实现判断树形→相应旋转，最后再进行常规的先序遍历。



代码

```python
# 
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
        self.height = 1

class AVL:
    def __init__(self):
        self.root = None

    def insert(self, value):
        if not self.root:
            self.root = Node(value)
        else:
            self.root = self._insert(value, self.root)

    def _insert(self, value, node):
        if not node:
            return Node(value)
        elif value < node.value:
            node.left = self._insert(value, node.left)
        else:
            node.right = self._insert(value, node.right)

        node.height = 1 + max(self._get_height(node.left), self._get_height(node.right))

        balance = self._get_balance(node)

        if balance > 1:
            if value < node.left.value:	# 树形是 LL
                return self._rotate_right(node)
            else:	# 树形是 LR
                node.left = self._rotate_left(node.left)
                return self._rotate_right(node)

        if balance < -1:
            if value > node.right.value:	# 树形是 RR
                return self._rotate_left(node)
            else:	# 树形是 RL
                node.right = self._rotate_right(node.right)
                return self._rotate_left(node)

        return node

    def _get_height(self, node):
        if not node:
            return 0
        return node.height

    def _get_balance(self, node):
        if not node:
            return 0
        return self._get_height(node.left) - self._get_height(node.right)

    def _rotate_left(self, z):
        y = z.right
        T2 = y.left
        y.left = z
        z.right = T2
        z.height = 1 + max(self._get_height(z.left), self._get_height(z.right))
        y.height = 1 + max(self._get_height(y.left), self._get_height(y.right))
        return y

    def _rotate_right(self, y):
        x = y.left
        T2 = x.right
        x.right = y
        y.left = T2
        y.height = 1 + max(self._get_height(y.left), self._get_height(y.right))
        x.height = 1 + max(self._get_height(x.left), self._get_height(x.right))
        return x

    def preorder(self):
        return self._preorder(self.root)

    def _preorder(self, node):
        if not node:
            return []
        return [node.value] + self._preorder(node.left) + self._preorder(node.right)

n = int(input().strip())
sequence = list(map(int, input().strip().split()))

avl = AVL()
for value in sequence:
    avl.insert(value)

print(' '.join(map(str, avl.preorder())))
```



代码运行截图 

![image-20240402195758227](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240402195758227.png)



### 02524: 宗教信仰

http://cs101.openjudge.cn/practice/02524/



思路：因为对并查集的应用并不熟悉，所以求助了GPT。这段代码首先定义了 `find_religion_limit` 来计算学校里可能的宗教数目上限。然后调用 `find_religion_limit` 函数来计算每组数据的宗教数目上限，并输出结果。



代码

```python
# 
def find_religion_limit(n, m, pairs):
    religions = {}  # 用字典来表示宗教信仰
    groups = list(range(n + 1))  # 初始化每个学生所在的宗教群组，初始状态下每个学生都是一个独立的群组

    def find_group(student):
        # 查找学生所在的宗教群组
        if groups[student] != student:
            groups[student] = find_group(groups[student])
        return groups[student]

    def merge_groups(student1, student2):
        # 合并两个宗教群组
        group1 = find_group(student1)
        group2 = find_group(student2)
        if group1 != group2:
            groups[group2] = group1

    for i, j in pairs:
        merge_groups(i, j)

    # 统计每个宗教群组的数量
    for student in range(1, n + 1):
        group = find_group(student)
        religions[group] = religions.get(group, 0) + 1

    return len(religions)

# 主程序
case_num = 1
while True:
    n, m = map(int, input().split())
    if n == 0 and m == 0:
        break

    pairs = [tuple(map(int, input().split())) for _ in range(m)]
    limit = find_religion_limit(n, m, pairs)
    print(f"Case {case_num}: {limit}")
    case_num += 1

```



代码运行截图 

![image-20240402201315751](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240402201315751.png)



## 2. 学习总结和收获

这周作业好难。感觉彻底上强度了，上周的作业靠自己花大量时间还可以自己写出了，顶多是没那么简洁，这周自己写的代码要么样例过了一直WA，要么就是逻辑在但是写不出来，最后只好看看题解，多少有点点挫败感。越来越长的代码也是一个挑战，有时候开头写得好好的，套了几个函数和循环脑子就乱了。

这周最大的教训就是写不出来得及时看题解/问GPT，不然后果就是消耗大量精力在已经有固定解法的题目上：）





