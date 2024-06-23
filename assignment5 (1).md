# Assignment #5: "树"算：概念、表示、解析、遍历

Updated 2124 GMT+8 March 17, 2024

2024 spring, Complied by 张惠雯，生命科学学院



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11 家庭中文版

Python编程环境：PyCharm Community Edition 2023.2.1

## 1. 题目

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/



思路：对于用类来实现树是熟练了一些，一开始卡在如何找到根节点的位置，如果双重遍历显然不现实，然后参考了一下题解的思路，发现可以一边存子节点一边更改parent作为是否有父节点的标签，感觉特别巧妙而且几乎没什么耗时和实现难度，就把这个方法记录到了cheatsheet里面。



代码

```python
#2300012302 张惠雯 
class tree:
    def __init__(self):
        self.left=None
        self.right=None

def tree_height(node):
    if node is None:
        return -1
    left_height=tree_height(node.left)
    right_height=tree_height(node.right)
    return max(left_height,right_height)+1




n=int(input())
Tree=[tree() for i in range(n)]
parent=[False]*n
k=0
def leaves(node):
    global k
    if node.left==node.right==None:
        k+=1
    if node.left!=None:
        leaves(node.left)
    if node.right!=None:
        leaves(node.right)
for i in range(n):
    l,r=map(int,input().split())
    if l!=-1:
        Tree[i].left=Tree[l]
        parent[l]=True
    if r!=-1:
        Tree[i].right=Tree[r]
        parent[r]=True
root=parent.index(False)
leaves(Tree[root])

print(tree_height(Tree[root]),k)




```



代码运行截图 

![image-20240324113136367](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240324113136367.png)



### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/



思路：上课的时候一边听讲一边写的代码，感觉还是对树的具体应用不够熟练，大概的思路是有的但是在具体实现的细节上面还是会卡住。这题的树是多叉的，所以需要一个列表来存储每个节点所有的子节点。从左到右遍历整棵树，遇到字母就进栈，如果栈不为空，就把字母加入到栈顶元素的子节点中。如果遇到左括号，说明栈顶元素此时可能有子节点，遇到右括号则从栈顶弹出一个元素。前序和后序遍历树的做法目前还没完全掌握，参考了题解的写法。



代码

```python
# 2300012302 张惠雯
class tree:
    def __init__(self,v):
        self.children=[]
        self.value=v

tr=input()
stack=[]
node=None
for i in tr:
    if i.isalpha():
        node=tree(i)
        if stack:
            stack[-1].children.append(node)


    elif i=='(':
        if node:
            stack.append(node)
            node=None#在遇到'('时，将当前节点压入栈中，并将当前节点设置为空，以准备处理下一个新的子树。
    elif i==')':
        if stack:
            node=stack.pop()
root=node

def pre(node):
    output=[node.value]
    for chi in node.children:
        output.extend(pre(chi))
    return ''.join(output)

def post(node):
    output=[]
    for chi in node.children:
        output.extend(post(chi))
    output.append(node.value)
    return ''.join(output)

print(pre(root))
print(post(root))




```



代码运行截图 



![image-20240325101212528](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240325101212528.png)

### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/



思路：这题是自己花了大半个上午死磕出来的，没有参考题解，因为感觉自己的思路和逻辑没有问题，其实最大的困难就是在输出上和接收输入的格式上。对于输入，先全部接收到列表里，然后再通过‘*’的个数来决定dataset的个数并进行遍历，每次遍历中如果遇到的是文件就直接放入当前栈顶的f子节点中，不用改成树的结构（因为文件不会有子节点，保持为str格式便于后续的字典序排列），如果遇到的是目录则加入栈顶的d子节点中，并且把其压入栈顶。可以将每次输出开头的root视作树根。

写完之后还挺有成就感的TT，因为验证了自己的逻辑没有问题，而且个人感觉这个逻辑还挺好理解的。



代码

```python
# 2300012302 张惠雯
class tree:
    def __init__(self,v):
        self.d=[]
        self.f=[]
        self.value=v


def output(node,n):
    for chi in node.d:
        print("|     " *n+ chi.value)
        output(chi,n+1)
    node.f.sort()
    for i in node.f:
        print('|     '*(n-1)+i)


a=[]
while True:
    try:
        a.append(input())
    except EOFError:
        break

data=a.count('*')
for i in range(data):
    root=tree('ROOT')
    stack=[]
    while a:
        j=a.pop(0)
        if j[0]=='f':

            if stack:
                stack[-1].f.append(j)
            else:
                root.f.append(j)
        elif j[0]=='d':
            j=tree(j)
            if stack:
                stack[-1].d.append(j)
            else:
                root.d.append(j)
            stack.append(j)
        elif j==']':
            stack.pop()
        elif j=='*' or j=='#':
            break

    print(f"DATA SET {i+1}:")
    print('ROOT')
    output(root,1)
    print()
```



代码运行截图 



![image-20240325122255657](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240325122255657.png)

### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/



思路：一开始是想着从后往前遍历的，样例都过了但是WA，后来发现从后往前便遍历会在碰到两个子节点分别为操作数和操作符的时候出现问题（也可能是我的思路出了问题），所以还是决定按照课件里的思路写。建树的思路还是挺直接的，根节点也很容易找到，就是遍历的方式上比较难想到，直接复用了题解的写法。



代码

```python
# 2300012302 张惠雯
low=['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z']
class Tree:
    def __init__(self,v):
        self.right=None
        self.left=None
        self.value=v
def level_order_traversal(root):
    queue = [root]
    traversal = []
    while queue:
        node = queue.pop(0)
        traversal.append(node.value)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return traversal



n=int(input())
for i in range(n):
    a=input()
    post=[]
    stack=[]
    for j in a:
        post.append(j)
    while post:
        char=post.pop(0)

        if char in low:
            char = Tree(char)
            stack.append(char)

        else:
            char=Tree(char)
            if stack:
                c1=stack.pop()
                c2=stack.pop()
                char.left=c2
                char.right=c1
                stack.append(char)
            if len(post)==0:
                root=char
    ans=level_order_traversal(root)[::-1]
    print(''.join(ans))


```



代码运行截图 

![image-20240325195108251](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240325195108251.png)



### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/



思路：课下花了不少时间才搞懂二叉树的三种遍历方式，但是搞懂了之后写题目的思路就比较清晰了。整体就是使用递归的思想，通过后序遍历最后一位是根节点的结论，不断把树分割为左子树和右子树，然后递归调用建树的函数进行操作。一开始本来想着先把树建好再用前序遍历，发现会遇到麻烦（把str转换为Tree形式后就没办法使用index函数直接找到根节点再中序表达式中的位置了）,所以尝试了直接构建前序表达式，发现可行而且思路更顺利。



代码

```python
# 2300012302 张惠雯
class Tree:
    def __init__(self,value):
        self.left=None
        self.right=None
        self.value=value

def build_tree(midtree,posttree,output):#递归函数
    root=posttree[-1]
    output.append(root)
    root_index=midtree.index(root)#将mid和post都切分为左子树和右子树
    mid_left_tree=midtree[0:root_index]
    mid_right_tree=midtree[root_index+1:len(midtree)]
    post_left_tree=posttree[0:len(mid_left_tree)]
    post_right_tree=posttree[len(mid_left_tree):len(posttree)-1]
    if post_left_tree:#递归调用
        build_tree(mid_left_tree,post_left_tree,output)
    if post_right_tree:
        build_tree(mid_right_tree,post_right_tree,output)
    return output


a=input()
mid=[]
for i in a:
    mid.append(i)
b=input()
post=[]

for i in b:
    post.append(i)


ans=build_tree(mid,post,[])
root=Tree(post[-1])

print(''.join(ans))



```



代码运行截图 

![image-20240326113650274](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240326113650274.png)

### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/



思路：有了前面一题的基础，写这题就快了很多，稍微修改一下思路就可以，前序转后序的一个需要注意的地方就是从右侧向左侧进行遍历，最终把输出reverse一下即可。



代码

```python
# 2300012302
class Tree:
    def __init__(self,value):
        self.left=None
        self.right=None
        self.value=value

def build_tree(midtree,pretree,output):#递归函数
    root=pretree[0]
    output.append(root)
    root_index=midtree.index(root)#将mid和pre都切分为左子树和右子树的
    mid_left_tree=midtree[0:root_index]
    mid_right_tree=midtree[root_index+1:len(midtree)]
    pre_left_tree=pretree[1:len(mid_left_tree)+1]
    pre_right_tree=pretree[len(mid_left_tree)+1:len(pretree)]

    if pre_right_tree:#post=pre先遍历右子树再遍历左子树再reverse：所以先遍历右子树
        build_tree(mid_right_tree,pre_right_tree,output)
    if pre_left_tree:
        build_tree(mid_left_tree, pre_left_tree, output)
    return output







while True:
    try:

        a=input()
        pre=[]
        for i in a:
            pre.append(i)
        b=input()
        mid=[]

        for i in b :
            mid.append(i)


        ans=build_tree(mid,pre,[])
        root=Tree(pre[0])

        ans.reverse()
        print(''.join(ans))
    except EOFError:
        break


```



代码运行截图 



![image-20240326121043909](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240326121043909.png)

## 2. 学习总结和收获

这周的ddl比较多，所以没有腾出时间做每日选做，但是作业题都是花了不少时间和功夫的，一边写一边把上课的课件都过了一遍，且基本上经过思考都能独立写出来。对于树的结构算是更加熟练了，基本上有思路就能写，感觉最重要的还是想清楚代码实现的原理。

树确实是一个很直观的数据结构，做作业的时候发现草稿纸很重要，有时候在脑子里想不明白的代码逻辑在纸上画几棵树就能搞明白了。





