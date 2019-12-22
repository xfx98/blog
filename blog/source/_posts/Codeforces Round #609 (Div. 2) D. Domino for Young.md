---
title: 'Codeforces Round #609 (Div. 2) D. Domino for Young'
date: 2019-12-22 15:12:24
tags: 
 - 栈
 - Java
categories:
 - 栈
---
[题目链接](http://codeforces.com/contest/1269/problem/D)
题意是给定一个图，每一列的高度是递减的，问能在这个图中放多少1*2或2*1大小的块多少个.
通过观察可发现，当两个`奇数高`列之间有偶数个`偶数高`的列，这些能够全部覆盖的，两侧合并之后对能够的到的最大数没有影响，仍然可以继续这样办。
所以只需记录前面是否有奇数高列，和此次奇数高列和前面一个（除了已经消去的奇数高列）之间的偶数高列是否为偶数，如果为偶数就能完全覆盖。类似于栈操作。
AC代码如下。

```java
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.io.StreamTokenizer;
import java.math.BigInteger;
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;
 
public class Main {
	static StreamTokenizer st = new StreamTokenizer(new BufferedInputStream(System.in));
	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static PrintWriter pr = new PrintWriter(new BufferedOutputStream(System.out));
	static Scanner sc = new Scanner(System.in);
 
	public static void main(String[] args) {
//		long tic = System.currentTimeMillis();
		int n = nextInt();
		int arr[] = new int[n];
		long ans = 0;
		boolean isodd = false;
		int odnum = 0;
		for (int i = 0; i < n; i++) {
			arr[i] = nextInt();
			ans += arr[i] / 2;
			if (arr[i] % 2 == 1 && isodd) {
				isodd = false;
				odnum -= 1;
				ans++;
			} else if (arr[i] % 2 == 1) {
				odnum++;
				isodd = true;
			} else {
				if (odnum > 0)
					isodd = !isodd;
				else 
					isodd = false;
			}
		}
		System.out.println(ans);
 
//		long toc = System.currentTimeMillis();
//		System.out.println("Elapsed time: " + (toc - tic) + " ms");
	}
 
	private static long A(long n, long k, long mod) {
		long jcn = n;
		for (int i = 1; i < k; i++) {
			jcn *= (n - i);
			jcn %= mod;
		}
		return jcn;
	}
 
	private static long C(long n, long k, long mod) {
		long jcn = n;
		long jck = 1;
		for (int i = 1; i < k; i++) {
			jcn *= (n - i);
			jcn %= mod;
			jck *= i;
			jck %= mod;
		}
		jck *= k;
		jck %= mod;
		return jcn * pow(jck, mod - 2, mod) % mod;
	}
 
	static long pow(long a, long b, long mod) {
		long result = 1;
		while (b > 0) {
			if (b % 2 == 1) {
				result = (result * a) % mod;
			}
			a = (a * a) % mod;
			b /= 2;
		}
		return result;
	}
 
	static int nextInt() {
		try {
			st.nextToken();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return (int) st.nval;
	}
 
	static double nextDouble() {
		try {
			st.nextToken();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return st.nval;
	}
 
	static String next() {
		try {
			st.nextToken();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return st.sval;
	}
 
	static long nextLong() {
		try {
			st.nextToken();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return (long) st.nval;
	}
}
```

