---
title: '取数游戏[dp+博弈]'
date: 2019-06-18 20:52:27
tags: 
 - dp
 - 博弈
 - Java
categories:
 - dp
 - 博弈
---

有如下一个双人游戏：N个正整数的序列放在一个游戏平台上，两人轮流从序列的两端取数，每次有数字被一个玩家取走后，这个数字被从序列中去掉并累加到取走该数的玩家的得分中，当数取尽时，游戏结束。以最终得分多者为胜。

编一个执行最优策略的程序，最优策略就是使自己能得到在当前情况下最大的可能的总分的策略。你的程序要始终为两位玩家执行最优策略。

输入第1行包括一个正整数N（2≤N≤100）, 表示序列中正整数的个数。输入第2行包含用空格分隔的N个正整数（1≤所有正整数≤200）。

只有一行，用空格分隔的两个整数: 依次为先取数玩家和后取数玩家的最终得分。

样例输入 


>6 
4 7 2 9 5 2

样例输出 

>18 11

以区间最优从而选出全部最优。当区间只有1个时，先手选取获得最优，两个便是较大的最优，同时也是区间和减去上一个状态中较小的，因为想要获得较大的数那么就要把较小的留给对手，gain[i][j] 是在[i,j] 中区间可取得的最大的值，如果要求gain[i][j+1]的最优便是将gain[i-1][j]和gain[i][j+1]中较小的给对手，这样先手便能得到最优值。
接下来给出AC代码，该代码由初始状态，计算第[j,j+i]区间的值，及由只有一个元素的区间得出有两个元素区间的最优，然后的出含有三个元素的最优......直到得出[1,n]区间的最优，java代码如下：

```java
import java.util.Scanner;
public class Main{
    public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int array[] = new int[n + 1];
		int sum[] = new int[n + 1];
		int gain[][] = new int[n + 1][n + 1];
		for (int i = 1; i <= n; i++) {
			array[i] = sc.nextInt();
			sum[i] += sum[i - 1] + array[i];
			gain[i][i] = array[i];
		}
		for (int i = 1; i < n; i++) {
			for (int j = 1; j < n-i+1; j++) {
				gain[j][j+i] = sum[j+i] - sum[j-1] - Math.min(gain[j+1][j+i], gain[j][j+i-1]);
			}

		}
		System.out.println(gain[1][n] + " " + (sum[n] - gain[1][n]));

	}
}
```
