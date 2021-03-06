#### Rabin-Karp 算法

Rabin-Karp 算法的思想：

​	假设子串的长度为 M (pat)，目标字符串的长度为 N (txt)

​	计算子串的 hash 值 hash_pat

​	计算目标字符串txt中每个长度为 M 的子串的 hash 值（共需要计算 N-M+1次）

​	比较 hash 值：如果 hash 值不同，字符串必然不匹配; 如果 hash 值相同，还需要使用朴素算法再次判断

#### KMP算法

KMP算法（Knuth-Morris-Pratt）的思想就是，当子串与目标字符串不匹配时，其实你已经知道了前面已经匹配成功那 一部分的字符（包括子串与目标字符串）。以阮一峰的文章为例，当空格与 D 不匹配时，你其实 知道前面六个字符是“ABCDAB”。KMP 算法的想法是，设法利用这个已知信息，不要把“搜索位置” 移回已经比较过的位置，继续把它向后移，这样就提高了效率。

#### 学习总结：

需要针对动态规划和字符串的相关题目进行反复练习



不同路径2的状态转移方程

```java
if (obstacleGrid[i][j] == 0) dp[i][j] = 0;
if (obstacleGrid[i][j] != 0) dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
```

