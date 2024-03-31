# 动态规划之最长上升子序列，最长公共子序列，LIS,LCS模板(java)

## LIS模板（最长上升子序列）

最长上升子序列，简称LIS。假设我们有一个序列bi，当b1<b2<…<bs的时候，我们称这个序列是上
升的。

对于给定的一个序列(a1,a2,…, aN)，我们也可以从中得到一些上升的子序列(ai1, ai2,., aik)，这里1<= i1<i2<…<iK<=N，但必须按照从前到后的顺序。比如，对于序列(1,7,3,5,9,4,8)，我们就会得到一些上升的子序列，如(1,7, 9),(3,4,8),(1,3,5,8)等等，而这些子序列中最长的(如子序列(1,3,5,8))，它的长度为4，因此该序列的最长上升子停列长度为4。

这是一个经典dp问题。子序列指的是数组中不一定连续但先后顺序一致的n个数组。

在求解LIS问题中，我们一般定义dp[i]表示以索引i结尾的最长子序列长度。

可以推出状态转移方程为

```
 if(dp[i]>dp[j]) dp[i]=Math.max(dp[j]+1,dp[i]);(0<=j<i)
 else dp[i]=Math.max(1,dp[i]);
```

时间复杂度为(n2)

> 蓝桥勇士
>
> ### 题目描述
>
> 小明是蓝桥王国的勇士，他晋升为蓝桥骑士，于是他决定不断突破自我。
>
> 这天蓝桥首席骑士长给他安排了*N* 个对手，他们的战力值分别为 a*1,*a*2,...,*a*n*，且按顺序阻挡在小明的前方。对于这些对手小明可以选择挑战，也可以选择避战。
>
> 作为热血豪放的勇士，小明从不走回头路，且只愿意挑战战力值越来越高的对手。
>
> 请你算算小明最多会挑战多少名对手。
>
> ### 输入描述
>
> 输入第一行包含一个整数 N，表示对手的个数。
>
> 第二行包含 N 个整数 a*1,*a*2,...,*an，分别表示对手的战力值。
>
> 1≤*N*≤10^3，1≤*ai*≤10^9。
>
> ### 输出描述
>
> 输出一行整数表示答案。
>
> ### 输入输出样例
>
> #### 示例
>
> > 输入
>
> ```txt
> 6
> 1 4 3 2 5 6
> ```
>
> > 输出
>
> ```txt
> 4
> ```
>
> ### 运行限制
>
> - 最大运行时间：1s
> - 最大运行内存: 256M

```
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt(); // 对手的个数
        int[] power = new int[n]; // 每个对手的战斗力
        for (int i = 0; i < n; i++) {
            power[i] = sc.nextInt();
        }

        // 创建一个数组dp，dp[i]表示挑战了前i个对手时的最多挑战次数
        int[] dp = new int[n];
        dp[0] = 1; // 第一个对手肯定可以挑战，所以挑战次数至少为1

        // 遍历每个对手，计算其可以挑战的最多次数
        for (int i = 1; i < n; i++) {
            dp[i] = 1; // 初始化为1，表示至少可以挑战一次

            // 检查前面的对手，如果当前对手的战斗力大于前面某个对手的战斗力，
            // 则可以在挑战这个对手的基础上再挑战当前对手，因此挑战次数加1
            for (int j = 0; j < i; j++) {
                if (power[i] > power[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }

        // 找出挑战次数最多的情况，即为答案
        int maxChallenges = 0;
        for (int i = 0; i < n; i++) {
            maxChallenges = Math.max(maxChallenges, dp[i]);
        }

        // 输出结果
        System.out.println(maxChallenges);
    }
}

```

## LCS(最长公共子序列)

LCS(最长公共子序列)是一个经典的DP模型。

LCS问题是给定两个序列A和B，求它们的最长公共子序列。

在求解LCS时，一般我们会设dp\[i][j]表示A[1~i]序列和B[1~j]序列中(不规定结尾)的最长公共子序列的长度，状态转移方程为: 

```
		int max=0;
		for(int i=1;i<=n;i++) {
			for(int j=1;j<=m;j++) {
				if(a[i-1]==b[j-1]) dp[i][j] = Math.max(dp[i-1][j-1]+1, dp[i][j]);
				else {
					dp[i][j] = Math.max(dp[i-1][j], dp[i][j]);
					dp[i][j] = Math.max(dp[i][j-1], dp[i][j]);
				}
				max = Math.max(max, dp[i][j]);
			}
		}
		System.out.println(max);	
```

解释一下:当a[i]=b[j]时，可以将他们插入到LCS的后面，使得长度变长1，当a[i]!=b[j]时，说明此时LCS不会延长，那就要从dp\[i-1][j]和dp\[i][j-1]中取最大的作为最长的长度。

例题：

> 最长公共子序列
>
> ### 题目描述
>
> 给定一个长度为 N 数组 a 和一个长度为 M的数组 b。
>
> 请你求出它们的最长公共子序列长度为多少。
>
> ### 输入描述
>
> 输入第一行包含两个整数 N*,*M*，分别表示数组 *a* 和 b 的长度。
>
> 第二行包含 N*个整数a*1,*a*2,...,an。
>
> 第三行包含 M 个整数b*1,*b*2,...,*bn。
>
> 1≤*N*,*M*≤10^3，1≤ai,b*i*≤10^9。
>
> ### 输出描述
>
> 输出一行整数表示答案。
>
> ### 输入输出样例
>
> #### 示例 1
>
> > 输入
>
> ```txt
> 5 6
> 1 2 3 4 5
> 2 3 2 1 4 5
> ```
>
> > 输出
>
> ```txt
> 4
> ```
>
> ### 运行限制
>
> - 最大运行时间：1s
> - 最大运行内存: 128M

```
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt(); // 数组 a 的长度
        int m = sc.nextInt(); // 数组 b 的长度

        // 读取数组 a 和 b
        int[] a = new int[n];
        int[] b = new int[m];
        for (int i = 0; i < n; i++) {
            a[i] = sc.nextInt();
        }
        for (int i = 0; i < m; i++) {
            b[i] = sc.nextInt();
        }

        // 创建二维数组 dp，dp[i][j] 表示数组 a 的前 i 个元素和数组 b 的前 j 个元素的最长公共子序列的长度
        int[][] dp = new int[n + 1][m + 1];

        // 计算最长公共子序列的长度
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (a[i - 1] == b[j - 1]) {
                    // 如果当前元素相等，则最长公共子序列长度加一
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    // 如果当前元素不相等，则取上方和左方的最大值
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        // 输出结果，最长公共子序列的长度即为 dp[n][m]
        System.out.println(dp[n][m]);
    }
}

```

