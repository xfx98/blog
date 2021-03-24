---
title: '牛客练习赛34 c题 little w and Segment Coverage'
date: 2018-12-20 16:43:17
tags:
 - Java
 - 差分区间
categories:
 - 差分区间
---
链接：https://ac.nowcoder.com/acm/contest/297/C
来源：牛客网


题目描述 
小w有m条线段，编号为1到m。

用这些线段覆盖数轴上的n个点，编号为1到n。

第i条线段覆盖数轴上的区间是L[i]，R[i]。

覆盖的区间可能会有重叠，而且不保证m条线段一定能覆盖所有n个点。

现在小w不小心丢失了一条线段，请问丢失哪条线段，使数轴上没被覆盖到的点的个数尽可能少，请输出丢失的线段的编号和没被覆盖到的点的个数。如果有多条线段符合要求，请输出编号最大线段的编号（编号为1到m）。

输入描述:
第一行包括两个正整数n，m(1≤n，m≤10^5)。
接下来m行，每行包括两个正整数L[i]，R[i](1≤L[i]≤R[i]≤n)。
输出描述:
输出一行，包括两个整数a b。
a表示丢失的线段的编号。
b表示丢失了第a条线段后，没被覆盖到的点的个数。
示例1
输入

>5 3
1 3
4 5
3 4

输出
>3 0

>说明
若丢失第1条线段，1和2没被线段覆盖到。
若丢失第2条线段，5没被线段覆盖到。
若丢失第3条线段，所有点都被线段覆盖到了。

示例2
输入
>6 2
1 2
4 5

输出
>2 4

>说明
若丢失第1条线段，1，2，3，6没被线段覆盖到。
若丢失第2条线段，3，4，5，6没被线段覆盖到。

这里用差分区间进行读入，储存线段覆盖情况，然后记录没有被覆盖到的地方，并找出覆盖值为1的情况。然后以此得出从 1-x 中覆盖为 1 的个数。就能得出结果啦
>java AC代码

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int m = sc.nextInt();
		int[] data = new int[n+2];
		int[] l = new int[m], r = new int[m];
		for (int i = 0; i < m; i++) {
			l[i] = sc.nextInt();
			r[i] = sc.nextInt();
			data[l[i]]++;
			data[r[i] + 1]--;
		}
		int sum = data[0];
		int oth=0;
		for (int i = 1; i <= n; i++) {
			sum += data[i];
			if (sum == 1) {
				data[i] = 1;
			} else if (sum == 0) {
				oth++;
			} else {
				data[i] = 0;
			}
		}
		for (int i = 1; i <= n; i++) {
			data[i] += data[i - 1];
		}
		int a = 0, b = 1000000;
		for (int i = 0; i < m; i++) {
			int xi = data[r[i]] - data[l[i]-1];
			if (xi <= b) {
				a = i;
				b = xi;
			}
		}
		System.out.println((a + 1) + " " + (b+oth));
	}
}

```

