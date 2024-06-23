# Assignment #4: 排序、栈、队列和树

Updated 0005 GMT+8 March 11, 2024

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

### 05902: 双端队列

http://cs101.openjudge.cn/practice/05902/



思路：因为感觉对类的写法还是很不熟悉，所以这次尝试自己用类写了一下双端队列，只有动手写了才会发现自己有哪些地方不明白，是一个查缺补漏的过程。



代码

```python
# 
class deque:
    def __init__(self):
        self.queue=[]

    def push(self,a):#进队
        self.queue.append(a)

    def post_out(self):
        self.queue.pop()

    def pre_out(self):
        self.queue.pop(0)

    def empty(self):
        if self.queue==[]:
            return False
        else:
            return True


t=int(input())
for i in range(t):
    p=deque()
    n=int(input())
    for j in range(n):
            t,x=map(int,input().split())
            if t==1:
                p.push(x)
            elif t==2:
                if x==0:
                    p.pre_out()
                elif x==1:
                    p.post_out()
    if p.empty():
        ans=[]
        for z in p.queue:
            ans.append(str(z))
        print(' '.join(ans))
    else:
            print('NULL')






```



代码运行截图 ==（至少包含有"Accepted"）==



![image-20240318104113700](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240318104113700.png)

### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/



思路：因为上课的时候老师提到过用栈来实现，所以思路很清晰，一开始犹豫了一下是从前往后遍历还是从后往前，发现从前往后容易出现逻辑错误，而且后进先出也更符合栈的性质，感觉这道题目天生就是为栈准备的。从后往前依次把数字放入栈中，遇到一个运算符就从栈顶弹出两个数字进行运算然后再将结果压入栈中。

代码

```python
# 
a=input().split()
s=[]#存储运算符
n=[]#存储数字
for i in range(len(a)):
    x=a.pop()
    if x=='+' or x=='-' or x=='*' or x=='/':
        s.append(x)
        y = n.pop()
        z = n.pop()
        k = s.pop()
        if k == '+':
            n.append(y + z)
        elif k == '-':
            n.append(y-z)
        elif k == '*':
            n.append(y * z)
        elif k == '/':
            n.append(y/z)

    else:
        n.append(float(x))

ans=n[0]
print("{:.6f}".format(ans))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240318213945656](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240318213945656.png)



### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/



思路：因为是经典代码，所以在自己花了半个小时理不清思路之后决定直接去看题解的算法。shunting yard算法其实非常好理解，就是自己独立思考的时候很难把逻辑理清楚。这周末时间有点忙，打算等到本周稍微轻松一点的时候尝试自己独立写一下这题的代码。



代码

```python
# 
def infix_to_postfix(expression):
    precedence = {'+':1, '-':1, '*':2, '/':2}
    stack = []
    postfix = []
    number = ''

    for char in expression:
        if char.isnumeric() or char == '.':
            number += char
        else:
            if number:
                num = float(number)
                postfix.append(int(num) if num.is_integer() else num)
                number = ''
            if char in '+-*/':
                while stack and stack[-1] in '+-*/' and precedence[char] <= precedence[stack[-1]]:
                    postfix.append(stack.pop())
                stack.append(char)
            elif char == '(':
                stack.append(char)
            elif char == ')':
                while stack and stack[-1] != '(':
                    postfix.append(stack.pop())
                stack.pop()

    if number:
        num = float(number)
        postfix.append(int(num) if num.is_integer() else num)

    while stack:
        postfix.append(stack.pop())

    return ' '.join(str(x) for x in postfix)

n = int(input())
for _ in range(n):
    expression = input()
    print(infix_to_postfix(expression))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



![image-20240318221358871](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240318221358871.png)

### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/



思路：一开始对题目的理解有问题，即“出栈顺序无要求”，结果一直WA，看了群里的讨论才明白题干表达的是可以一边入栈一边出栈。自己一开始写的时候就老老实实按照题目的逻辑写，结果TLE了，参考了题解，但是觉得题解的思路略有些繁琐，所以自己在题解的基础上写了一个代码，顺利AC.

代码

```python
# 
def valid(a,b):
    if len(a)!=len(b):
        return False
    else:
        s=[]
        a=list(a)
        i=0
        while i<len(b):
            if s==[] and len(a)>0:
                s.append(a.pop(0))
            elif s[-1]!=b[i] and len(a)>0:
                s.append(a.pop(0))
            elif s[-1]==b[i]:
                s.pop()
                i+=1
            elif s[-1]!=b[i] and len(a)==0:
                return False
        return True

x=input()
while True:
    try:
        y=input()
        if valid(x,y):
            print('YES')
        else:
            print('NO')
    except EOFError:
        break

```



代码运行截图 

![image-20240318122206409](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240318122206409.png)



### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/



思路：在上课的时候一边听讲一边照着写的，感觉对于类的代码就是能够看明白，但是自己动手写还是有困难。



代码

```python
# 
class Treenode:
    def __init__(self):
        self.left=None
        self.right=None

def tree_depth(node):
    if node is None:
        return 0
    left_depth=tree_depth(node.left)
    right_depth=tree_depth(node.right)
    return max(left_depth,right_depth)+1




n=int(input())
Node=[Treenode() for i in range(n)]
for i in range(n):
    left,right=map(int,input().split())
    if left!=-1:
        Node[i].left=Node[left-1]
    if right!=-1:
        Node[i].right=Node[right-1]
root=Node[0]
ans=tree_depth(root)
print(ans)




```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



![image-20240312172110674](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240312172110674.png)

### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/



思路：一开始读完题目觉得很清楚，就直接写了冒泡排序，然后不出意外地超时了。尝试过自己思考实现归并排序，但是花了很多时间发现还是不行，所以就直接参考了题解。感觉这题真的有点难，看懂题解都花了不少功夫。



代码

```python
# 
def merge_sort(lst):
    # The list is already sorted if it contains a single element.
    if len(lst) <= 1:
        return lst, 0

    # Divide the input into two halves.
    middle = len(lst) // 2
    left, inv_left = merge_sort(lst[:middle])
    right, inv_right = merge_sort(lst[middle:])

    merged, inv_merge = merge(left, right)

    # The total number of inversions is the sum of inversions in the recursion and the merge process.
    return merged, inv_left + inv_right + inv_merge

def merge(left, right):
    merged = []
    inv_count = 0
    i = j = 0

    # Merge smaller elements first.
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            merged.append(left[i])
            i += 1
        else:
            merged.append(right[j])
            j += 1
            inv_count += len(left) - i #left[i~mid)都比right[j]要大，他们都会与right[j]构成逆序对，将他们加入答案

    # If there are remaining elements in the left or right half, append them to the result.
    merged += left[i:]
    merged += right[j:]

    return merged, inv_count

while True:
    n = int(input())
    if n == 0:
        break

    lst = []
    for _ in range(n):
        lst.append(int(input()))

    _, inversions = merge_sort(lst)
    print(inversions)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



![image-20240319165456813](C:\Users\ARENA TANG\AppData\Roaming\Typora\typora-user-images\image-20240319165456813.png)

## 2. 学习总结和收获

这周的作业比之前的难度明显高一些（至少是对于我来说），也花了差不多上周两倍的时间。首先是复习了类的写法，去b站又看了几个讲解类的视频，然后自己尝试用类写了deque并且很快顺利AC，很开心，对自己写类也不再恐惧了。其次是对于栈的使用，上学期写波兰表达式很困难，但是这次只花了十来分钟调试了一下就顺利AC了，说明之前讲栈的时候还是学到了新东西，熟练度也增加了。调度场没有自己写出来，但是看完能明白思路，并且对着文字的思路能够自己写出来代码，感觉问题也不大。主要就是最后一题有点超出自己范围，花了很多时间还是没有AC，意识到之前学排序的时候只是理解了各个排序的基本逻辑方法，对于用代码实现和其相关的适用情况其实还是没有掌握，可能在本周抽时间再复习一下各个排序的代码。





