# 位运算

| 运算符                        | 用法                   | 描述                                                                              |
| :---------------------------- | :--------------------- | :-------------------------------------------------------------------------------- |
| 按位与（ AND）<img width=75/> | `a & b`<img width=65/> | 对于每一个比特位，只有两个操作数相应的比特位都是 1 时，结果才为 1，否则为 0。     |
| 按位或（OR）                  | `a | b`                | 对于每一个比特位，当两个操作数相应的比特位至少有一个 1 时，结果为 1，否则为 0。   |
| 按位异或（XOR）               | `a ^ b`                | 对于每一个比特位，当两个操作数相应的比特位有且只有一个 1 时，结果为 1，否则为 0。 |
| 按位非（NOT）                 | `~ a`                  | 反转操作数的比特位，即 0 变成 1，1 变成 0。                                       |
| 左移（Left shift）            | `a << b`               | 将 `a` 的二进制形式向左移 `b` (< 32) 比特位，右边用 0 填充。                      |
| 有符号右移                    | `a >> b`               | 将 a 的二进制表示向右移`b`(< 32) 位，丢弃被移出的位。                             |
| 无符号右移                    | `a >>> b`              | 将 a 的二进制表示向右移`b`(< 32) 位，丢弃被移出的位，并使用 0 在左侧填充。        |

**优先级**
| 加减 | 移位 | 比较大小 | 位与 | 异或 | 位或
| :---------- | :----- | :------- | :------- | :------- | :------- |
| `+,-` | `<<,>>` | `<,>,==,!=`| `&` | `^` | `|` |

## 枚举二进制子集

针对二进制中为 `1` 的位做减法，

```text
例如 k = 10101，子集为：
10101
10100
10001
10000
00101
00100
00001
00000
```

**参考代码**

```cpp
int sub = k;
do {
  sub = (sub - 1) & k;
} while (sub != k);
```

**相关题目：**[LeetCode 1178. 猜字谜](https://leetcode-cn.com/problems/number-of-valid-words-for-each-puzzle/)

## 二进制状态压缩

指将一个长度为 $m$ 的 `bool` 数组用一个 $m$ 位二进制整数表示并存储的方法。

下列位运算操作实现原 `bool` 数组中对应下标元素的存取：

| 操作                                                         | 运算                 |
| :----------------------------------------------------------- | :------------------- |
| 取出整数 $n$ 在二进制表示下的第 $k$ 位                       | `(n >> k) & 1​`      |
| 取出整数 $n$ 在二进制表示下的第 $0 \sim k-1$ 位（后 $k$ 位） | `n & ((1 << k) - 1)` |
| 把整数 $n$ 在二进制表示下的第 $k$ 位取反                     | `n ^ (1 << k)`       |
| 对整数 $n$ 在二进制表示下的第 $k$ 位赋值 $1$                 | `n | (1 << k)`       |
| 对整数 $n$ 在二进制表示下的第 $k$ 位赋值 $0$                 | `n & (~(1 << k))`    |

> 当 m 较大时，可使用 C++ STL 中的 bitset 实现。

**相关题目：**

- [最短 Hamilton 路径](https://www.acwing.com/problem/content/93/)
- [LeetCode 1349. 参加考试的最大学生数](https://leetcode-cn.com/problems/maximum-students-taking-exam/)

## $lowbit$ 运算

$lowbit(n)$ 定义为非负整数 $n$ 在二进制表示下 “最低位的 $1$ 及其后边所有的 $0$” 构成的数值。例如 $n=10$ 的二进制表示为 $(1010)_2$，则 $lowbit(n)=2=(10)_2$。

**推导过程**

设 $n>0$，$n$ 的第 $k$ 位是 $1$，第 $0\sim k-1$ 位都是 $0$。

首先把 $n$ 取反，此时第 $k$ 位变为 $0$，第 $0\sim k-1$ 位变成 $1$。再另 $n=n+1$，此时因为进位，第 $k$ 位变为 $1$，第 $0\sim k-1$ 位都是 $0$

在上面取反加 $1$ 操作后，$n$ 的第 $k+1$ 位到最高位都与原来相反，故 $n~\&~(\textit{～} n + 1)$ 仅有第 $k$ 位为 $1$，其余位都是 $0$。对一个数负数求补码等于原码取反加一，在补码表示下 $-n=\textit{～}n+1$，故：

$$
lowbit(n)=n~\&\;(\textit{～}n+1)=n\;\&\;(-n)
$$