# DSAinPython
---------------------------------------------
## 13.3 Matrix Chain-Product: dynamic Programming
1. 引入问题
2. 分析
3. 实现

## 1. 引入问题
---------------------------------------------
提高矩阵乘法的效率：计算$A_{2\times 10} \* B_{10 \times 50} \* C_{50 \times 20}$.  $(AB)C$的计算量是$2\*50\*10+2\*20\*50=3000$,  
$A(BC)$的计算量是 $10\*20\*50+2\*20\*10=10400$。
    
也就是说：不同的相乘顺序，其计算量是不同的。  

**问题推广到一般情况**：
给定$A=A_0 * A_1* ... * A_{n-1}$, 其中$A_i$的大小为:$d_i \times d_{i+1}$.选择计算顺序(也就是在不同的地方加括号)，使得计算量最小。

> 计算量：the total number of scalar of multiplications

## 2. 分析
----------------------------------------
### 2.1 Brute-force
> The set of all different parenthesizations of the expression for A is equal in number to **the set of all different ferent binary trees that have n leaves**, i.e., **Catalan_number**.

![](https://github.com/zhouqp631/DSAinPython/blob/master/catalan.png)

> Catalan_number

**The number is exponential in n**，so....
### 2.2 Split the problem into subproblmes
:one: 无论在计算$A=A_0 * A_1* ... * A_{n-1}$时怎么加括号，都会出现这种形式$(A_0 * ... A_i) * (A_{i+1} * ... * A_{n-1})$.那么$A$的计算量为：$N_{0,i}:(A_0 * ... A_i)$+$N_{i+1,n-1}:(A_{i+1} * ... * A_{n-1})$+$f(0,i,n):d_0 * d_{i+1} * d_n$.

$A$的最优计算量为i从0到n-2遍历后得到的最小计算量，也就是：$N_{0,n-1} = min \lbrace N_{0,i} + N_{i+1,n-1}+d_0 * d_{i+1} * d_n \rbrace $.

> $N_{0,i}$表示计算$A_0 * ... A_i$的最小计算量 

:two: 递推的表达式已经有了，现在解决子问题的最优解。
  无论在计算$(A_0 * ... A_i)$时怎么加括号，都会出现这种形式$(A_0 * ... A_j) * (A_{j+1} * ... * A_{i})$.  
  那么$(A_0 * ... A_i)$的计算量为：$N_{0,j}:(A_0 * ... A_j)$+$N_{j+1,i}:(A_{j+1} * ... * A_{i})$+$f(0,j,i):d_0 * d_{j+1} * d_i$.  
  
  $(A_0 * ... A_i)$的最优计算量为i从0到n-2遍历后得到的最小计算量，也就是：
  $N_{0,i} = min \lbrace N_{0,j} + N_{j+1,i} + d_0 * d_{j+1} * d_i \rbrace $.

:three: 所以不断这样迭代，应该就可以得到base case：$N_{i,i}=0$.

- 总结：求解 $N_{0,n-1}$,递推公式为

![](https://github.com/zhouqp631/DSAinPython/blob/master/ditui.png)
## 3. 实现
----------------------------------------
- 初始化 $N_{i,i}=0$ for i in range(0,n)

![](https://github.com/zhouqp631/DSAinPython/blob/master/dp1.png)
> 初始化

- 然后可以计算 $N_{i,i+1}$，因为 $N_{i,i}$和 $N_{i+1,i+1}$都知道了，$A_i * A_{i+1}$只有一种计算方式。

![](https://github.com/zhouqp631/DSAinPython/blob/master/dp2.png)
> 第一轮迭代

- 然后计算 $N_{i,i+2}$

![](https://github.com/zhouqp631/DSAinPython/blob/master/dp3.png)
> 第二轮迭代

- 然后计算 $N_{i,i+3}$

- Finally， we get $N_{0,n-1}$

```python
def matrix_chain(d):
    ''' d is a list: [d[0],d[1],...,d[n-1],d[n]]
    '''
    n = len(d)-1
    N = [[0]*n for _ in range(n)]
    for b in range(1,n):      # number of products in subchain
        for i in range(n-b):  # start of subchain
            j = i+b
            N[i][j] = min(N[i][k]+N[k+1][j]+d[i]*d[k+1]*d[j+1] for k in range(i,j))
    return N[-1][-1]

```
## 3. 计算复杂度 :bullettrain_side:
----------------------------------------
3 nested loop: **[for b]** + **[for j]** + **[for k]**  
每一个迭代最多计算 $n$次，所以计算复杂度是 $O(n^3)$

## Main References
- Data Structures & Algorithms in Python
- [Catalan_number](https://en.wikipedia.org/wiki/Catalan_number)
