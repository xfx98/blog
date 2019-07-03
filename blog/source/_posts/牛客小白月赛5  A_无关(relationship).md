---
title: '牛客小白月赛5  A_无关(relationship)'
date: 2019-04-16 19:31:18
tags:
 - Java
 - 容斥原理
categories:
 - 容斥原理
---
链接：https://ac.nowcoder.com/acm/contest/135/A
来源：牛客网

题目描述 
  若一个集合A内所有的元素都不是正整数N的因数，则称N与集合A无关。

  给出一个含有k个元素的集合A={a1,a2,a3,...,ak}，求区间[L,R]内与A无关的正整数的个数。
  保证A内的元素都是素数。
输入描述:
>输入数据共两行：
第一行三个正整数L,R,k，意义如“题目描述”。
第二行k个正整数，描述集合A，保证k个正整数两两不相同。
#### 输出描述:
输出数据共一行：
第一行一个正整数表示区间[L,R]内与集合A无关的正整数的个数
#### 示例1
输入

> 1 10 4
2 3 5 7

输出

>1
#### 示例2
输入

>2 10 4
2 3 5 7

 输出
>0
#### 说明
>对于30%的数据：1<=L<=R<=10^6
对于100%的数据：1<=L<=R<=10^18,1<=k<=20,2<=ai<=100

这题可以用容斥获得与N有关的 个数
这里要注意集合A的乘积会爆 long 需要特判一下
接下来给出AC代码

```java
import java.io.BufferedInputStream;
import java.io.IOException;
import java.io.StreamTokenizer;
import java.util.Arrays;
import java.util.Date;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Scanner;
 
public class Main {
    @SuppressWarnings("deprecation")
    static StreamTokenizer st = new StreamTokenizer(new BufferedInputStream(System.in));
 
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        long l = sc.nextLong();
        long r = sc.nextLong();
        int t = sc.nextInt();
        int rc[] = new int[t];
        for (int i = 0; i < t; i++) {
            rc[i] = sc.nextInt();
        }
        l--;
        long cont1 = 0;
        for (int i = 1; i < (1 << t); i++) {
            int cont = 0;
            long te = 1;
            for (int j = 0; j < t; j++) {
                if (((i) & (1 << j)) != 0) {
                    cont++;
                    te *= rc[j];
                    if(te>r) {
                        break;
                    }
                }
            }
            if ((cont & 1) == 1) {
                cont1 -= l / te;
                cont1 += r / te;
            } else {
                cont1 += l / te;
                cont1 -= r / te;
            }
 
        }
        System.out.println(r-l-cont1);
    }
}
```

另外还有一种dfs容斥的方法

```java
import java.io.BufferedInputStream;
import java.io.IOException;
import java.io.StreamTokenizer;
import java.util.Arrays;
import java.util.Date;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Scanner;

public class Main {
	@SuppressWarnings("deprecation")
	static StreamTokenizer st = new StreamTokenizer(new BufferedInputStream(System.in));
	static long L, R, ans;
	static int num, k, A[] = new int[30];

	static void dfs(int p, long pro, int n) {
		if (p > k && n != 0) {
			if ((n & 1) != 0)
				ans += R / pro - (L - 1) / pro;
			else
				ans += (L - 1) / pro - R / pro;
		}
		if (p <= k) {
			dfs(p + 1, pro, n);
			if (R / pro >= A[p])
				dfs(p + 1, pro * A[p], n + 1);
		}
	}

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		L = sc.nextLong();
		R = sc.nextLong();
		k = sc.nextInt();
		for (int i = 1; i <= k; i++) {
			A[i] = sc.nextInt();
		}
		dfs(1, 1, 0);
		System.out.println(R - L + 1 - ans);
	}
}

```

