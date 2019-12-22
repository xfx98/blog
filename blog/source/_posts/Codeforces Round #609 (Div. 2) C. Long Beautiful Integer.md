---
title: 'Codeforces Round #609 (Div. 2) C. Long Beautiful Integer'
date: 2019-12-22 16:06:29
tags: 
 - 大数
 - Java
categories:
 - 大数
---

[题目链接](http://codeforces.com/contest/1269/problem/C)
题意是给定长为n的整数x，然后需要你找到不小于n并且这个数的第i位等于i+k最小的数。
很明显，可以通过把当前数x的前k位，当做循环节，查看形成的数是否大于数x，如果小于，只需把循环节加上1就能够保证形成的数大于x了，小于输出就行，这里要注意是末尾为9的时候，要有进位，当然不会出现循环节全是9加一的情况，因为全是9的形成的数一定不会小于x的。
java大数异常方便，直接加一（手动狗头）
AC代码
```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.BufferedReader;
import java.io.IOException;
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
		int n = sc.nextInt();
		int k = sc.nextInt();
		String s = sc.next();
		String s2 = s.substring(0,k);
		boolean flag = true;
		for(int i = k;i<n;i++) {
			if(s.charAt(i)<s2.charAt((i)%k)) {
				break;
			}else if(s.charAt(i)>s2.charAt((i)%k)) {
				flag = false;
				break;
			}
		}
		StringBuffer sb = new StringBuffer();
		if(flag) {
			for(int i = 0;i<n;i++) {
				sb.append(s2.charAt(i%k));
			}
		}else{
			String s3 = new BigInteger(s2).add(new BigInteger("1")).toString();
			for(int i = 0;i<n;i++) {
				sb.append(s3.charAt(i%k));
			}
		}
		System.out.println(sb.length());
		System.out.println(sb);
		
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

