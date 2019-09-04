---
title: 'The Preliminary Contest for ICPC Asia Nanjing 2019 B_super_log'
date: 2019-09-04 20:31:18
tags:
 - Java
 - 欧拉降幂
categories:
 - 欧拉降幂
---
## [super_log](https://nanti.jisuanke.com/t/41299)
题意是有个一函数log a*(x) ,x<1时为-1，x>1为1+log a*(log a(x))
给你a，b，m让求最小使不等式（log a*(x)>=b）成立的最小x%m
这题可以转换为求a的a次方的a次方......b个，模于m。
求幂可以用快速幂，然后必须降幂，因为m不一定是素数，所以这里就要用广义欧拉降幂来降幂了，否则幂就会因为太大而出错。
广义欧拉降幂需要求欧拉函数。这里用的筛法求的。

**如有不懂欢迎提问。**

java代码

```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.io.StreamTokenizer;
import java.util.Scanner;

public class Main {
	static StreamTokenizer st = new StreamTokenizer(new BufferedInputStream(System.in));
	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static PrintWriter pr = new PrintWriter(new BufferedOutputStream(System.out));
	static Scanner sc = new Scanner(System.in);
	static int N = 1000005;
	static int prime[] = new int[N], phi[] = new int[N], cnt = 0;
	static int n = 1000000;
	static boolean vis[] = new boolean[N];

	static void make_phi() {
		vis[0] = vis[1] = true;
		for (int i = 2; i <= n; i++) {
			if (!vis[i]) {
				prime[++cnt] = i;
				phi[i] = i - 1;
			}
			for (int j = 1; j <= cnt && prime[j] * i <= n; j++) {
				vis[i * prime[j]] = true;
				if (i % prime[j] == 0) {
					phi[i * prime[j]] = phi[i] * prime[j];
					break;
				} else
					phi[i * prime[j]] = phi[i] * (prime[j] - 1);
			}
		}
	}

//		long tic = System.currentTimeMilis();
//		long toc = System.currentTimeMillis();
//		System.out.println("Elapsed time: " + (toc - tic) + " ms");
	public static void main(String[] args) {
		make_phi();
		int T = nextInt();
		while (T-- > 0) {
			int a = nextInt();
			int b = nextInt();
			int m = nextInt();
			if (b == 0) {
				System.out.println(1 % m);
			} else {
				System.out.println(f(a, b, m));
			}
		}
	}

	static long f( long a, long b,long mod) {
		if (mod == 1)
			return 0;
		if (b == 0) {
			return 1;
		}
		long p = f(a, b - 1,phi[(int) mod]);
		if (p >= phi[(int) mod] || p == 0)
			return pow(a, p + phi[(int) mod], mod);
		else
			return pow(a, p, mod);
	}
	static long pow(long a, long b, long mod) {
		if (b == 0)
			return 1;
		if (b == 1)
			return a % mod;
		if ((b & 1) == 0)
			return pow(a * a % mod, b >> 1, mod) % mod;
		else
			return a * pow(a * a % mod, b >> 1, mod) % mod;
	}

	static long gcd(long a, long b) {
		if (b == 0) {
			return a;
		} else {
			return gcd(b, a % b);
		}
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


