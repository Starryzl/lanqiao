# 2021年第十二届蓝桥杯javaA组省赛

# 试题 A: 相乘
本题总分：5 分

【问题描述】

> 小蓝发现，他将 1 至 1000000007 之间的不同的数与 2021 相乘后再求除以1000000007 的余数，会得到不同的数。小蓝想知道，能不能在 1 至 1000000007 之间找到一个数，与 2021 相乘后再除1000000007 后的余数为 999999999。如果存在，请在答案中提交这个数；如果不存在，请在答案中提交 0。

【答案提交】

这是一道结果填空的题，你只需要算出结果后提交即可。本题的结果为一个整数，在提交答案时只填写这个整数，填写多余的内容将无法得分。

------

思路：签到题，直接暴力就可

答案：17812964

```
public class Main {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
	for(long i=1;i<1000000007;i++) {
		if(i*2021%1000000007==999999999) {
			System.out.println(i);
		}
	}
}
}
```



# 试题 B: 直线
本题总分：5 分

【问题描述】

> 在平面直角坐标系中，两点可以确定一条直线。如果有多点在一条直线上，那么这些点中任意两点确定的直线是同一条。
>
> 给定平面上 2 × 3 个整点 {(x, y)|0 ≤ x < 2, 0 ≤ y < 3, x ∈ Z, y ∈ Z}，即横坐标是 0 到 1 (包含 0 和 1) 之间的整数、纵坐标是 0 到 2 (包含 0 和 2) 之间的整数的点。这些点一共确定了 11 条不同的直线。
>
> 给定平面上 20 × 21 个整点 {(x, y)|0 ≤ x < 20, 0 ≤ y < 21, x ∈ Z, y ∈ Z}，即横坐标是 0 到 19 (包含 0 和 19) 之间的整数、纵坐标是 0 到 20 (包含 0 和 20) 之间的整数的点。请问这些点一共确定了多少条不同的直线。

【答案提交】

这是一道结果填空的题，你只需要算出结果后提交即可。本题的结果为一个整数，在提交答案时只填写这个整数，填写多余的内容将无法得分。

------

分析：

我们需要计算在给定平面上由 20 行 21 列的整点所确定的不同直线数量。

1. 首先，我们可以枚举所有可能的点
2. 对于每一对点 (x1, y1) 和 (x2, y2)，我们计算其斜率为 (y2 - y1) / (x2 - x1)。
3. 为了确保斜率的分子和分母是互质的，我们使用辅助函数求最大公约数（gcd）来简化斜率的表示。
4. 将简化后的斜率和截距拼接成字符串，并将其添加到一个集合中，以确保不重复计算相同的直线。
5. 最后，我们得到的集合中的元素数量就是不同直线的数量。
6. 最后需要加上水平和竖直线的数量，即 20（竖直线）和 21（水平线）。
7. 输出结果为不同直线的数量加上水平和竖直线的数量的和

答案：40257


```
import java.util.Scanner;
import java.util.HashSet;
import java.util.Set;

// 类名必须为 Main
public class Main {
    public static void main(String[] args) {
        Set<String> lines = new HashSet<>(); // 存储不同直线的集合

        // 循环确定两个点
        for(int x1 = 0; x1 < 20; x1++) {
            for(int x2 = 0; x2 < 20; x2++) {
                for(int y1 = 0; y1 < 21; y1++) {
                    for(int y2 = 0; y2 < 21; y2++) {
                        if(x1 == x2 || y1 == y2 ) {
                            continue; // 排除相同的点
                        }
                        
                        // 计算直线的斜率和截距
                        String line = "y = ";
                        int y = y2 - y1; 
                        int x = x2 - x1;
                        int d = gcd(x, y); // 求最大公约数
                        String k = y / d + "/" + x / d; // 斜率
                        line += k + "x + " ;
                        
                        //将两点带入由此两点确定的直线y=kx+b中：
                        //联立y1 = k*x1 +b,y2 = k*x2 +b，消得b*(x2-x1) = y2*x1-y1*x2
                        int m = y2 * x1 - y1 * x2; // 截距的分子
                        int n = gcd(m, x); // 求最大公约数
                        line += m / n + "/" + x / n; // 截距--直线与 y 轴的交点的坐标值
                        lines.add(line); // 添加直线到集合
                    }
                }
            }
        }
        
        // 最后答案要加上 k = 0 及 b = 0 的情况，即横线和竖线
        System.out.println(lines.size() + 21 + 20);
    }
    
    // 辅助方法：求最大公约数
    public static int gcd(int x, int y) {
        return x % y == 0 ? y : gcd(y, x % y);
    }
}

```

提问：为什么求斜率需要求最大公约数？

> 答：在求斜率时，我们需要确保斜率的表示是最简形式，即分子和分母互质。这是因为，如果斜率的分子和分母不互质，它们可以约分为一个更小的整数比例，这样就得到了相同的直线，但是在我们的问题中，我们希望计算的是不同的直线数量。
>
> 例如，考虑两点 (2, 4) 和 (4, 8)。它们的斜率为 (8 - 4) / (4 - 2) = 4 / 2 = 2。然而，这两个点实际上在同一条直线上，因为它们的斜率可以约分为 1。
>
> 因此，我们需要确保斜率的分子和分母是互质的，即它们没有共同的约数，以保证我们计算的是不同的直线数量。而求最大公约数的目的就是为了将分子和分母约分到最简形式。



# 试题 C: 货物摆放
本题总分：10 分

【问题描述】

> 小蓝有一个超大的仓库，可以摆放很多货物。
>
> 现在，小蓝有 n 箱货物要摆放在仓库，每箱货物都是规则的正方体。小蓝规定了长、宽、高三个互相垂直的方向，每箱货物的边都必须严格平行于长、宽、高。
>
> 小蓝希望所有的货物最终摆成一个大的立方体。即在长、宽、高的方向上分别堆 L、W、H 的货物，满足 n = L × W × H。
>
> 给定 n，请问有多少种堆放货物的方案满足要求。
>
> 例如，当 n = 4 时，有以下 6 种方案：1×1×4、1×2×2、1×4×1、2×1×2、 2 × 2 × 1、4 × 1 × 1。
>
> 请问，当 n = 2021041820210418 （注意有 16 位数字）时，总共有多少种方案？
>
> 提示：建议使用计算机编程解决问题。

【答案提交】

这是一道结果填空的题，你只需要算出结果后提交即可。本题的结果为一个整数，在提交答案时只填写这个整数，填写多余的内容将无法得分。

分析：见代码注释

答案：2430

注：本题详解来自蓝桥用户Olah_的题解分享[0货物摆放 - 蓝桥云课 (lanqiao.cn)](https://www.lanqiao.cn/problems/1463/learning/)。并在此基础上进行了进行了简单的小优化与注释

```
import java.util.ArrayList;

public class Main {
    public static void main(String args[]) {

        //常规思路

        /*
        long num = 2021041820210418l;

        int count = 0;
        for ( long i = 1 ; i < num ; i++ ){
            for ( long j = 1 ; j < num ; j++ ){
                for ( long k = 1 ; k < num ; k++ ){
                    if ( i * j *um ){
                        count++;
                    }
                }
            }
        }
        */

        //显而易见，这个算法的时间复杂的非常的大。计算机想要算出结果，可能需要几天的时间

        //所以要另辟蹊径

        //想想，三个数相乘要等于num，那么这三个数是不是都是num的因子，都能被num整除呢？
        //上方的算法，三层for循环都是从1遍历到num，其中很多组合都是无效的
        //简而言之，这些无效组合中不能同时被num整除，而有效的组合，任何一个数都能被num整除
        //所以，如果我们能从num的因子里面去找组合，是不是时间复杂度就要小许多？

        long num = 2021041820210418l;
        //如果直接赋值,计算机默认数字是int类型，会报错。所以要在后面加'l'，转换为long类型

        //定义一个ArrayList数组，存放num的因子
        ArrayList<Long> arr = new ArrayList<>();

        //从1开始遍历，遍历到num的平方根结束。不需要把num遍历一遍，这样算法复杂都也非常大，重点看for循环里面的语句
        for ( long i = 1 ; i <= Math.sqrt(num) ; i++ ){

            if ( num % i == 0 ){
                arr.add(i);     //如果能被整除，就放到arr数组中

                //！！！重点在这里，当i能被num整除的情况下，求出num关于i的另外一个除数n
                //这样，for循环不需要从1遍历到num。可以通过较小的因子，求出另外一个较大的因子
                long n = num / i;

                //如果num = Math.sqrt(num)*Math.sqrt(num),那么由较小的因子求较大的因子时，会重复，要排除这种情况
                if ( n != i ){      //当然，此时num，不会出现这种情况。如果num=4，就会出现这种情况
                    arr.add(n);     //将较大的因子放入arr数组
                }

            }

        }

        //System.out.println(arr.size());   //num一共有128个因子

        //不需要三层for循环，求出两数及可得第三数
        int count = 0;
         for(long i: arr) {
        	for(long j: arr) {
        			long k = num/i/j;//如果不能整除（为小数），k向下取整，则不能i*j*k == num，说明此三数不能为一组
            		if(i*j*k == num) {
            			count++;
        		}
        		
        	}
        }
        System.out.println(count);

    }
}
```



# 试题 D: 路径
本题总分：10 分

【问题描述】

> 小蓝学习了最短路径之后特别高兴，他定义了一个特别的图，希望找到图
> 中的最短路径。
> 小蓝的图由 2021 个结点组成，依次编号 1 至 2021。
> 对于两个不同的结点 a, b，如果 a 和 b 的差的绝对值大于 21，则两个结点
> 之间没有边相连；如果 a 和 b 的差的绝对值小于等于 21，则两个点之间有一条
> 长度为 a 和 b 的最小公倍数的无向边相连。
> 例如：结点 1 和结点 23 之间没有边相连；结点 3 和结点 24 之间有一条无
> 向边，长度为 24；结点 15 和结点 25 之间有一条无向边，长度为 75。
> 请计算，结点 1 和结点 2021 之间的最短路径长度是多少。
> 提示：建议使用计算机编程解决问题。

【答案提交】
这是一道结果填空的题，你只需要算出结果后提交即可。本题的结果为一个整数，在提交答案时只填写这个整数，填写多余的内容将无法得分。

答案：10266837



```
import java.util.*; // 导入 Java 的工具包，包括集合等

public class Main { // 主类名为 Main
    public static void main(String[] args) { // 主方法
        int n = 2022; // 初始化变量 n 为 2022
        int[] q = new int[n]; // 创建一个长度为 n 的整数数组 q
        q[1] = 0; // 初始化 q[1] 为 0

        // 将数组 q 中除了 q[1] 之外的元素初始化为整数的最大值
        for (int i = 2; i <= 2021; i++) {
            q[i] = Integer.MAX_VALUE;
        }

        // 动态规划
        // 当前 q[j] 表示从 1~j 的最短距离
        // q[j] 可以是当前 1~j 的最短距离或者前一状态到该点的最短距离
        for (int i = 1; i <= 2020; i++) {
            for (int j = i + 1; j <= 2021 && (j - i <= 21); j++) {
                q[j] = Math.min(q[j], q[i] + le(i, j)); // 更新 q[j] 的值为当前值和前一状态到该点的最短距离的较小值
            }
        }

        System.out.println(q[2021]); // 输出 q[2021] 的值
    }

    // 辅助方法，计算两个数的最大公约数
    public static int gcd(int a, int b) {
        return b != 0 ? gcd(b, a % b) : a;
    }

    // 辅助方法，计算两个数的最小公倍数
    public static int le(int a, int b) {
        return a * b / gcd(a, b); // 返回 a 和 b 的最小公倍数
    }
}

```

# 试题 E: 回路计数
本题总分：15 分

【问题描述】
>蓝桥学院由 21 栋教学楼组成，教学楼编号 1 到 21。对于两栋教学楼 a 和 b，当 a 和 b 互质时，a 和 b 之间有一条走廊直接相连，两个方向皆可通行，否
>则没有直接连接的走廊。
>小蓝现在在第一栋教学楼，他想要访问每栋教学楼正好一次，最终回到第
>一栋教学楼（即走一条哈密尔顿回路），请问他有多少种不同的访问方案？两个
>访问方案不同是指存在某个 i，小蓝在两个访问方法中访问完教学楼 i 后访问了
>不同的教学楼。
>提示：建议使用计算机编程解决问题。

【答案提交】
这是一道结果填空的题，你只需要算出结果后提交即可。本题的结果为一个整数，在提交答案时只填写这个整数，填写多余的内容将无法得分。

答案：881012367360

```
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;

/**
 * 这种写法要跑好几秒才能得出结果
 * @author HQL
 *
 */
public class _回路计数 {

    static ArrayList<Integer>[] list; // 用于存储每个教学楼连接的其他教学楼
    static Map<String, Long> set = new HashMap<String, Long>(); // 用于存储已经计算过的状态及其对应的结果

    public static void main(String[] args) {
        // 初始化数组
        list = new ArrayList[21 + 1];
        for (int i = 0; i < list.length; ++i)
            list[i] = new ArrayList<Integer>();
        // 根据互质关系建立连接
        for (int i = 1; i <= 21; ++i)
            for (int j = i + 1; j <= 21; ++j)
                if (gcd(i, j) == 1) {
                    list[i].add(j);
                    list[j].add(i);
                }
        // 搜索满足条件的哈密尔顿回路的数量并输出结果
        System.out.println(dfs(21, 1, 0));
    }

    // 深度优先搜索
    public static long dfs(int total, int cur, int m) {
        // 如果所有教学楼都被访问过，则找到一个哈密尔顿回路，返回 1
        if (total == 0)
            return 1;
        long res = 0;
        // 遍历当前教学楼连接的所有目标教学楼
        for (int p : list[cur]) {			
            // 如果目标教学楼未被访问过，则继续递归搜索
            if ((m & (1 << p)) != (1 << p)) {
                String pm = p + "-" + m;
                long r = 0;
                // 使用哈希表进行剪枝，避免重复计算
                if (set.containsKey(pm)) {		
                    r = set.get(pm);
                } else {
                    if (p != 1 || p == 1 && total == 1)
                        r = dfs(total - 1, p, m | (1 << p));
                    set.put(pm, r);
                }
                res += r;
            }
        }
        return res;
    }

    // 求最大公约数
    public static int gcd(int a, int b) {
        if (a == 0 || b == 0)
            return 0;
        return a % b == 0 ? b : gcd(b, a % b);
    }
}

```



`1 << p` 表示将数字 1 向左移动 `p` 位，即进行左移运算。这个运算的结果是得到一个二进制数，其中只有第 `p` 位是 1，其他位都是 0。

为什么要检查第 `p` 位的状态是否为 1 呢？因为在这段代码中，通过使用位运算操作来记录教学楼的访问状态。如果某个教学楼被访问过，就在状态中的相应位置标记为 1；否则，该位置为 0。因此，通过检查第 `p` 位是否为 1，可以判断教学楼 `p` 是否已经被访问过。

# 试题 F: 最少砝码
时间限制: 1.0s 内存限制: 512.0MB 本题总分：15 分

【问题描述】

> 你有一架天平。现在你要设计一套砝码，使得利用这些砝码可以称出任意小于等于 N 的正整数重量。那么这套砝码最少需要包含多少个砝码？注意砝码可以放在天平两边。

【输入格式】

输入包含一个正整数 N。

【输出格式】

输出一个整数代表答案。

【样例输入】

7

【样例输出】

3

【样例说明】

> 3 个砝码重量是 1、4、6，可以称出 1 至 7 的所有重量。
> 1 = 1；
> 2 = 6 − 4 (天平一边放 6，另一边放 4)；
> 3 = 4 − 1；
> 4 = 4；
> 5 = 6 − 1；
> 6 = 6；
> 7 = 1 + 6；
> 少于 3 个砝码不可能称出 1 至 7 的所有重量。



分析：看了大家的题解，感觉这题还是得找规律，看代码注释

```
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        long value = scanner.nextLong();
        scanner.close();
        long n = 1;// 初始砝码数量为1
        long total_w=1;// 初始总重量为1
        long weight = 1;// 初始砝码重量为1
        while(total_w<value) {// 循环直到总重量大于等于目标重量
        	n++;
        	weight *=3;// 下一个砝码的重量是上一个砝码重量的3倍
        	total_w +=weight;
        }
        System.out.println(n);
    }
}
```

