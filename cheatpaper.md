## 序列

切片：

```python
list[a:b:c]
#a为起始下标，b为结束下标，c为步长，左开右闭
```

tips：通过切片可以获得原列表的一个副本，这样后续更改一个列表的内容时不会影响其他副本的值

排序：

```python
list=list.sort()#从小到大（按照字典序，如果元素是序列，则从第一个往后依次比较）
list=list.sort(reverse=True)#从大到小
```

插入：

```python
list.insert(i,x)#表示在列表的第i索引处插入x，插入后x的索引就是i
```

查找某元素数量：

```python
num1=list.count(a)
```

查找某一元素的下标：

```python
str.find(a,b,c)

list.index(a,b,c)
```

其中a表示要找的元素，b表示起始索引，c表示终止索引，左开右闭.

区别：find只适用于字符串，index适用于列表和字符串。

find找不到会返回-1，index找不到会报ValueError

## 输出/输入

**同一行输出**：

使用end=’’，引号中间填写希望中间隔开的东西，比如说一个空格

在下一行print()从而达到换行的目的

```python
for y in range(m):
    print(long1[x][y],end=" ")
print()
```

保留小数的多种方式：

```python
print("{:.2f}".format(3.146)) # 3.15
print(round(3.123456789,5)# 3.12346四舍六入五成双
#保留n位小数：
print(f’{x:.nf}’)
```

不定行输入：**套用try-except循环**

 

Try except 无法使用break来跳出循环，如果已知结束的特定输入（比如说输入0代表结束），

那么可以用以下代码：

```python
while True:
	try:
		Xxxxx
		If input()==’0’:
		break
	except EOFFError:
		break
```





## 矩阵：

1. 基本方法：列表套列表，用i，j两个指针
2. 将n*n的矩阵用某一元素进行填充的格式：

e.g：a是列数，b是行数

​	matrix = [[1]*a for _ in range(b)]

3. 双重循环防止index out of range：下界使用max（0，a），上界使用min（n，b），n为矩阵长。注意：range是左开右闭，列表的指针是0-n-1

4. 可以套一层保护圈，如最外圈包一层0，减轻考虑循环的时间

   

   

   

## 其他

```python

float(“inf”)#表示正无穷，可以用于赋值

output = ','.join(my_list)
print(output)#把列表中的所有元素一串输出，条件是列表中的元素都是str类型

mylist=[1,2,3,4,5]
print(mylist,sep=',')#把列表中所有元素之间用sep内部的东西分开，输出形式仍然是一个列表[1, 2, 3, 4, 5]

ord（）#返回对应的ASCII表值，chr（）返回ASCII值对应的字符
bin(),oct(),hex()#分别表示二进制，八进制，十六进制的转换

# 二进制转十进制
binary_str = "1010"
decimal_num = int(binary_str, 2) # 第一个参数是字符串类型的某进制数，第二个参数是他的进制，最终转化为整数
print(decimal_num)  # 输出 10


#Dp写不出来用递归，为了防止爆栈（re）：使用缓存
from functools import lru_cache
@lru_cache(maxsize=128)
#注意：使用的时候函数的参数需要是不可修改的，即不可以为列表字典等，可以改用元组（）或者set

#判断完全平方数
import math
def isPerfectSquare(num):
    if num < 0:
        return False
    sqrt_num = math.isqrt(num)
    return sqrt_num * sqrt_num == num
print(isPerfectSquare(97)) # False

#年份calendar
import calendar
print(calendar.isleap(2020)) # True，判断是否是闰年

#字符串的连接
str=str1+str2

#用同一个数填充整个列表
dp=[[0]*(i+1) for i in range(5)]
#输出[[0], [0, 0], [0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0, 0]]

#str相关
upper(self, /)
    Return a copy of the string converted to uppercase.

lower(self, /)

replace(self, old, new, count=-1, /)
    Return a copy with all occurrences of substring old replaced by new.
```

## 算法相关

**1.质数筛**

```python
#欧拉筛 输出素数列表
def Euler_sieve(n):
    primes = [True for _ in range(n+1)]
    p = 2
    while p*p <= n:
        if primes[p]:
            for i in range(p*p, n+1, p):
                primes[i] = False
        p += 1
    primes[0]=primes[1]=False
    return primes
print(Euler_sieve(20))
# [False, False, True, True, False, True, False, True, False, False, False, True, False, True, False, False, False, True, False, True, False]

#埃氏筛 输出素数列表
N=20
primes = []
is_prime = [True]*N
is_prime[0] = False;is_prime[1] = False
for i in range(1,N):
    if is_prime[i]:
        primes.append(i)
        for k in range(2*i,N,i): #用素数去筛掉它的倍数
            is_prime[k] = False
print(primes)
# [2, 3, 5, 7, 11, 13, 17, 19]

#欧拉筛 直接判断某个数是否是质数
2.欧拉筛
prime=[]
N=n（最大的质数范围）
is_prime = [True] * (n + 1)  # 初始化为全是素数

def euler_sieve():     

    for i in range(2, n//2+1):
        if is_prime[i]:
            primes.append(i)
        for j in prime: # 将所有质数的倍数标记为非素数
If i*j>=N:
break
is_prime[j*i] = False
If i%j==0:
break#如果i是某个质数的倍数,那么停止循环（所有该质数的倍数已经被标记过了）

# 测试
euler_sieve()
n=int(input())
If is_prime[n]:
print(YES)#对应位置是True 说明是质数 是False说明不是质数

```

**2.二分查找:**

```python
import bisect
sorted_list = [1,3,5,7,9] #[(0)1, (1)3, (2)5, (3)7, (4)9]
position = bisect.bisect_left(sorted_list, 6)#查找某一元素插入后的索引
print(position)  # 输出：3，因为6应该插入到位置3，才能保持列表的升序顺序

bisect.insort_left(sorted_list, 6)#将某个元素插入原列表并不改变升序/降序
print(sorted_list)  # 输出：[1, 3, 5, 6, 7, 9]，6被插入到适当的位置以保持升序顺序

sorted_list=(1,3,5,7,7,7,9)
print(bisect.bisect_left(sorted_list,7))
print(bisect.bisect_right(sorted_list,7))
# 输出：3 6
#右侧插入，如果有相同元素，就输出最大的索引，左侧输入则相反
```

3. heap 优先队列：

   特别地，使用了heap结构之后该列表就只能用heap包里的函数，不能再用remove，append等

   ```python
   import heapq # 优先队列可以实现以log复杂度拿出最小（大）元素
   lst=[1,2,3]
   heapq.heapify(lst) # 将lst优先队列化，获得最小堆，从小到大排序
   heapq.heappop(lst) # 从队列中弹出树顶元素，实际上是从前往后弹（默认最小，相反数reverse一下lst）
   heapq.heappush(lst,element) # 把元素压入堆中
   top_value = heap[0]#查看最小值但不弹出
   ```

4. **default_dict：**

   defaultdict是Python中collections模块中的一种数据结构，它是一种特殊的字典，可以为字典的值提供默认值。当你使用一个不存在的键访问字典时，defaultdict会自动为该键创建一个默认值，而不会引发KeyError异常。

   defaultdict的优势在于它能够简化代码逻辑，特别是在处理字典中的值为可迭代对象的情况下。通过设置一个默认的数据类型，它使得我们不需要在访问字典中不存在的键时手动创建默认值，从而减少了代码的复杂性。

   使用defaultdict时，首先需要导入collections模块，然后通过指定一个默认工厂函数来创建一个defaultdict对象。一般来说，这个工厂函数可以是int、list、set等Python的内置数据类型或者自定义函数。

   ```python
   from collections import defaultdict
   # 创建一个defaultdict，值的默认工厂函数为int，表示默认值为0
   char_count = defaultdict(int)
   # 统计字符出现次数
   input_string = "hello"
   for char in input_string:
       char_count[char] += 1
   print(char_count)  # 输出 defaultdict(<class 'int'>, {'h': 1, 'e': 1, 'l': 2, 'o': 1})）
   print(char_count['h'])
   #输出：1
   
   dict.get(key,default=None) 
   # 其中，my_dict是要操作的字典，key是要查找的键，default是可选参数，表示当指定的键不存在时要返回的默认值
   ```

5. **DFS深度搜索**

   ```python
   #例题：迷宫的所有路径
   n,m=map(int,input().split())
   ma=[]
   
   for _ in range(n):
       ma.append(list(map(int,input().split())))
   ans=0
   def dfs(ma,i,j):
       global ans#要在函数内部设置全局变量，方便更新ans的值
       if i==n-1 and j==m-1:
           ans+=1
           #走到终点一次就增加一个ans
   
       elif ma[i][j]==0:
           dx=[0,1,-1,0]
           dy=[1,0,0,-1]
           for k in range(4):
               if 0<=i+dx[k]<n and 0<=j+dy[k]<m:
                   ma[i][j]=1
                   #修改为墙，防止往回走
                   dfs(ma,i+dx[k],j+dy[k])
                   #往四面八方走
           ma[i][j]=0
           #还原为初始值
   dfs(ma,0,0)
   print(ans)
   
   
   #八皇后
   def is_safe(board, row, col):
       # Check if there is a queen in the same column
       for i in range(row):
           if board[i] == col:
               return False
           # Check diagonals
           if abs(board[i] - col) == row - i:
               return False
       return True
   
   def solve_queens(n):
       def dfs(queens, row):
           if row == n:
               result.append(queens[:])  # Found a solution, add to result
               return
           for col in range(n):
               if is_safe(queens, row, col):
                   queens.append(col)
                   dfs(queens, row + 1)
                   queens.pop()  # Backtrack
   
       result = []
       dfs([], 0)
       return result
   
   # Test the function
   solutions = solve_queens(8)
   solutions.sort()
   n=int(input())
   ans=[]
   for _ in range(n):
       b=int(input())
       ans.append(solutions[b-1])
   
   for i in ans:
       for a in range(8):
           i[a]=i[a]+1
       print(''.join(map(str,i)))
   
   ```



6.BFS广度优先搜索

```python
# 最大连通域面积
#使用了bfs的思想遍历整个棋盘，在数过W后将其标记为‘.’，确保后续不会再重复数这个元素。同时，在周围没有W的情况下，因为队列无法弹出会自动终止循环，不需要额外添加一个参数来计算，这样子会使代码更加简洁。
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
                            
            if ans > 0:
                re.append(ans)
                ans = 0

    re.append(ans)
    result.append(max(re))

for z in result:
    print(z)
    
#数字操作：给定一个整数n，计算从1开始每次可以*2或者+1，最少经历多少次才可以得到n


from collections import deque

def minOperations(target):
    queue = deque([(1, 0)])  # (当前数字，操作次数)
    
    while queue:
        num, operations = queue.popleft()
        
        if num == target:  # 如果当前数字等于目标数字，返回操作次数
            return operations
        
        # 尝试加1和乘2两种操作
        next_add = num + 1
        next_mul = num * 2
        
        # 将加1和乘2得到的数字加入队列
        queue.append((next_add, operations + 1))
        queue.append((next_mul, operations + 1))

    return -1  # 如果无法得到目标数字，返回-1

# 读取输入并调用函数输出结果
target = int(input())
print(minOperations(target))

#最短迷宫路径
from collections import deque
n,m=map(int,input().split())
ma=[]
result=float('inf')
for i in range(n):
    ma.append(list(map(int,input().split())))
def bfs(ma):
    global result
    queue = deque([(0, 0, 0)])
    visited = set()  # 用集合记录已经访问过的位置
    while queue:
        i, j, k = queue.popleft()  # 从左侧弹出队列中的第一个元素
        if i == n - 1 and j == m - 1:
            result = min(result, k)  # 更新最短路径长度
        else:
            dx = [1, 0, 0, -1]
            dy = [0, 1, -1, 0]
            if ma[i][j] == 0:
                for _ in range(4):
                    x, y = i + dx[_], j + dy[_]
                    if 0 <= x < n and 0 <= y < m and (x, y) not in visited:
                        visited.add((x, y))  # 标记位置(x, y)为已访问，防止重复访问导致RE，TLE
                        queue.append((x, y, k + 1))  # 将符合条件的位置加入队列
bfs(ma)
if result!=float('inf'):
    print(result)
else:
    print(-1)





```

bfs常用数据结构：deque，类似列表，但是更高效

相关操作和作用：

```python
from collections import deque
d = deque()  # 创建一个空的deque对象

d.append(x)  # 在deque的右端添加元素x

d.appendleft(x)  # 在deque的左端添加元素x

d.pop()  # 从deque的右端删除并返回最右边的元素

d.popleft()  # 从deque的左端删除并返回最左边的元素

len(d)  # 返回deque中元素的数量
d.clear()  # 清空deque中的所有元素

d = deque(maxlen=5)  # 创建一个最大长度为5的deque，当超出长度时，自动弹出最左边元素

list(d)  # 将deque转换为列表


```



DP动态规划

```python
#斐波那契数列 一维数组 up-down类型 递归写法
dp=[-1 for i in range(200)]#初始化一个数组
dp[0]=0#设置好初始值
dp[2]=1
dp[1]=1
n=int(input())
def f(x):

    if dp[x-1]!=-1 and dp[x-2]!=-1:
        dp[x]=dp[x-1]+dp[x-2]#如果数组里面已经有了，就直接加
    else:
        f(x-1)#如果没有，就需要递归
        dp[x] = dp[x - 1] + dp[x - 2]
for i in range(n):
    a=int(input())
    if a>2:#注意边界情况，避免指针出现负值
        f(a)
    print(dp[a])

#数字三角形 bottom-up类型 
n=int(input())
tri=[]
dp=[]
for i in range(n):
    tri.append(list(map(int,input().split())))
    dp.append([0]*(i+1))
for i in range(n):#先把最后一行更新为tri里面的
    dp[n-1][i]=tri[n-1][i]
for z in range(n-2,-1,-1):
    for k in range(z+1):#运用贪心策略，每次加底下两个中比较大的数，存储在上一行的相应位置
        dp[z][k]=max(dp[z+1][k],dp[z+1][k+1])+tri[z][k]
print(dp[0][0])#最后输出顶端的就是所求的最大数

#数字三角形 top-down类型
from functools import lru_cache 

@lru_cache(maxsize = 128) 
def f(i, j):
    if i == N-1:
        return tri[i][j]
	return max(f(i+1, j), f(i+1, j+1)) + tri[i][j]#和斐波那契数列思路差不多

N = int(input())
tri = []
for _ in range(N):
    tri.append([int(i) for i in input().split()])
print(f(0, 0))


#最大连续子序列和 在dp库里找最大值即可
n = int(input())
a = list(map(int, input().split()))

dp = [0]*n
dp[0] = a[0]

for i in range(1, n):
    dp[i] = max(dp[i-1]+a[i], a[i])#状态转移方程：只有两种可能，取较大的

print(max(dp))

#最长上升子序列的长度
n = int(input())
b = list(map(int, input().split()))
dp = [1]*n

for i in range(1, n):
    for j in range(i):
        if b[j] < b[i]:
            dp[i] = max(dp[i], dp[j]+1)#dp对应索引存储的是以第i个数为结尾的上升子序列长度，对于i之前的数，如果小于这个，就可以更新dp[i]的值

print(max(dp))


#小偷背包

n,b=map(int, input().split())
price=[0]+[int(i) for i in input().split()]
weight=[0]+[int(i) for i in input().split()]
bag=[[0]*(b+1) for _ in range(n+1)]
#bag[i][j] 表示前 i 件物品放入容量为 j 的背包可以获得的最大价值。
#price[i] 表示第 i 件物品的价值。
#weight[i] 表示第 i 件物品的重量。
for i in range(1,n+1):
    for j in range(1,b+1):
        if weight[i]<=j:
            bag[i][j]=max(price[i]+bag[i-1][j-weight[i]], bag[i-1][j])
        else:
            bag[i][j]=bag[i-1][j]
#状态转移方程的意义是在考虑第 i 件物品时，背包的容量为 j 时的最大价值取决于两种情况的较大值
#1，第 i 件物品放入背包，其价值为 price[i]，并且需要腾出 weight[i] 的空间，此时背包容量变为 j-weight[i]，因此最大价值为 price[i] + bag[i-1][j-weight[i]]。
#2，不放入第 i 件物品，背包容量为 j 时的最大价值为 bag[i-1][j]，即前 i-1 件物品放入容量为 j 的背包时的最大价值。
#通过状态转移方程，逐步计算出放入不同数量和重量的物品时，背包在不同容量下可以获得的最大价值，最终得到的结果就是整个问题的解。
print(bag[-1][-1])


#采药 类似小偷背包
# 首先将草药按顺序存入列表，然后构造一个二维dp表，i表示第i个草药，j表示此时剩余的时间数，如果下一个草药的时间比j小，那么就分两种情况，放入或者不放入该草药。如果下一个草药的时间比t大，那么就只能不放入这个草药。对i和j进行双循环遍历，最后到第M组的第T次即为所求
T, M = map(int, input().split())  # T是总时间，M是草药数目
herbal = []
for _ in range(M):
    t, v = map(int, input().split())
    herbal.append([t, v])

dp = [[0] * (T + 1) for _ in range(M + 1)]

for a in range(1, M + 1):
    for b in range(1, T + 1):
        if herbal[a - 1][0] <= b:  # 如果草药采摘时间小于等于剩余时间
            dp[a][b] = max(dp[a-1][b], dp[a-1][b-herbal[a-1][0]] + herbal[a-1][1])
        else:
            dp[a][b] = dp[a-1][b]

print(dp[M][T])




```



## math库

```python
#开头要写
import math
math.sqrt() #开方
math.ceil()#取浮点数上整数
math.floor()#取浮点数的下整数
math.gcd(a,b)#取两个数的最大公约数
math.pow(2,3) # 8.0 幂运算
math.inf#表示正无穷
math.log(100,10) # 2.0  
#math.log(x,base) 以base为底，x的对数
math.comb(5,3) # 组合数，C53
math.factorial(5) # 5！阶乘
```



### **collection库**

```python
#counter：计数字典子类，适用于list，str
#可以生成一个字典，其中key是对应的元素，value是该元素的出现次数
from collections import Counter
# 从可迭代对象创建 Counter
my_list = [1, 2, 3, 1, 2, 3, 4, 5]
my_counter = Counter(my_list)
print(my_counter)
# 输出: Counter({1: 2, 2: 2, 3: 2, 4: 1, 5: 1}

#也可直接自定义通过关键字参数创建 Counter
my_counter_explicit = Counter(a=2, b=3, c=1)
print(my_counter_explicit)
# 输出: Counter({'b': 3, 'a': 2, 'c': 1})

#特别地，访问不存在的元素不会引发 KeyError，而会返回 0
count_of_6 = my_counter[6]
print(count_of_6)
# 输出: 0

#常用的一些操作
# 获取所有元素
elements = my_counter.elements()
print(list(elements))
# 输出: [1, 1, 2, 2, 3, 3, 4, 5]
# 获取最常见的 n 个元素及其计数，会返回一个列表，其中包含计数最多的前 n 个元素及其计数。每个元素都表示为一个元组，第一个元素是元素本身，第二个元素是它在 Counter 对象中的计数。
most_common = my_counter.most_common(2)
print(most_common)
# 输出: [(1, 2), (2, 2)]
# 更新计数，即增加这些元素
my_counter.update([1, 2, 3, 4])
print(my_counter)
# 输出: Counter({1: 3, 2: 3, 3: 3, 4: 2, 5: 1})
```



## 其他捞分小tips

1.除法是否得到整数？（4/2=2.0）

2.缩进错误

3.用于调试的print是否删去

4.非一般情况的边界条件

5.递归中的return的位置

6.贪心是否最优

7.正难则反（剪绳子→拼绳子）

8.审题是否准确？是否漏掉输出？

9.考试地址：http://cs101.openjudge.cn 



