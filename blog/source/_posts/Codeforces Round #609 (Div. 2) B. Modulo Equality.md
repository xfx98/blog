---
title: 'Codeforces Round #609 (Div. 2) B. Modulo Equality'
date: 2019-12-22 15:50:24
tags: 
 - 暴力
 - Java
categories:
 - 暴力
---
[题目链接](http://codeforces.com/contest/1269/problem/B)
题意是给两个数列，问使数列一的所有数加上一个正整数，然后模于m后能变成数列二。
这题主要排序后暴力即可，由于被hack了，所有总结下出现的错误。
这里要注意：

 - 一个数ans如果满足条件那么m-ans 也可能满足条件。注意只是可能
 - 如果通过所有数的差加上m模于m得出的数得到的值k，可能这个数k不符合条件，但是m-k一点符合条件。
 - 还有就是得到的值k和m-k都符合条件，但是对位并不一样的情况，或者多个符合如
 ```
2 10
4 9
1 6
```
这样需要找出全部符合的，取最小值了。。。
AC代码java版
```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.io.StreamTokenizer;
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
		int n = sc.nextInt();
		int m = sc.nextInt();
		int aa[] = new int[n];
		int ab[] = new int[n];
		for(int i = 0;i<n;i++) {
			aa[i] = sc.nextInt();
		}
		for(int i = 0;i<n;i++) {
			ab[i] = sc.nextInt();			
		}
		Arrays.sort(aa);
		Arrays.sort(ab);
		int te = 2000000000;
		//2100
		//2110
        int ans = te;
		for(int i = 0;i<n;i++) {
			te = (aa[i]-ab[0]+m)%m;
			boolean flag = true;
			for(int j =0;j<n;j++) {
				if((aa[(i+j)%n]-ab[j]+m)%m!=te&&aa[(i+j)%n]-ab[j]!=te) {
					flag = false;
					break;
				}
			}
			if(flag) {
				boolean test = true;
                
                te = Math.min(te,m-te);
				for(int k = 0;k<n;k++) {
					if((aa[(i+k)%n]+te)%m!=ab[k]) {
						test = false;
					}
				}
				if(test) {
					ans = Math.min(te,ans);
				}else {
					ans = Math.min(m-te,ans);
				}
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

