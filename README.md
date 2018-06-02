# DSAinPython
---------------------------------------------
### Matrix Chain-Product: dynamic Programming
1. 引入问题
2. 分析
3. 实现

#### 1. 引入问题
----------------------------------------
提高矩阵乘法的效率：计算$A_{2\times 10} \* B_{10 \times 50} \* C_{50 \times 20}$，一种方法 $(AB)C$的计算量是$2\*50\*10+2\*10\*50=3000$,另一种计算方法$A(BC)$的计算量是 $10\*20\*50+2\*20\*10=10400$。
也就是说：不同的相乘顺序，其计算量是不同的。
现将问题推广到一般情况：给定$A=A_0 * A_1*... A_{n-1}$,选择计算顺序，使得计算量最小。

> 计算量：the total number of scalar of multiplications

#### 2. 分析
----------------------------------------


#### 3. 实现
----------------------------------------




### Main References
- Data Structures $&$ Algorithms in Python
