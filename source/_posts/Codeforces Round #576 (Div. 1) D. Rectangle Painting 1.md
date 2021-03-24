---
title: 'Codeforces Round #576 (Div. 1) D. Rectangle Painting 1'
date: 2019-08-31 13:21:27
tags: 
 - dp
 - Java
categories:
 - dp
---
### D. Rectangle Painting 1
>time limit per test1 second
memory limit per test256 megabytes
inputstandard input
outputstandard output

There is a square grid of size n×n. Some cells are colored in black, all others are colored in white. In one operation you can select some rectangle and color all its cells in white. It costs max(h,w) to color a rectangle of size h×w. You are to make all cells white for minimum total cost.

### Input
>The first line contains a single integer n (1≤n≤50) — the size of the square grid.
Each of the next n lines contains a string of length n, consisting of characters '.' and '#'. The j-th character of the i-th line is '#' if the cell with coordinates (i,j) is black, otherwise it is white.

### Output
>Print a single integer — the minimum total cost to paint all cells in white.

### Examples
input

    3
    ###
    #.#
    ###
output

    3

input

    3
    ...
    ...
    ...

output

    0

inputCopy

    4
    #...
    ....
    ....
    #...

output

    2

input

	5
	#...#
	.#.#.
	.....
	.#...
	#....
output

	5

题意为有一个方形的区域，有白块和黑块，一次可以将一个矩形区域内的所有方块变成白色，代价为max（m,n）,即矩形的长边。求最小代价。
这个问题可以使用dp解决。主要就是将这个大问题分解成小问题。f[x][y][z][w]表示使（x,y）,(z,w)两点之间的区域全变成白色所需要的最低代价。
这里将整个问题不断分解成两个小问题，试探所有可能，来求区域最小。递归的方法比较节省代码，同时更容易理解。
```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.io.StreamTokenizer;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Scanner;
import java.util.Stack;
 
public class Main {
	static StreamTokenizer st = new StreamTokenizer(new BufferedInputStream(System.in));
	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static PrintWriter pr = new PrintWriter(new BufferedOutputStream(System.out));
	static Scanner sc = new Scanner(System.in);
 
//		long tic = System.currentTimeMillis();
//		long toc = System.currentTimeMillis();
//		System.out.println("Elapsed time: " + (toc - tic) + " ms");
	static int f[][][][] = new int[55][55][55][55];
	static String s[];
 
	public static void main(String[] args) {
		int n = sc.nextInt();
		s = new String[n+3];
		for (int i = 1; i <= n; i++) {
			s[i] = " "+sc.next();
		}
		for (int i = 0; i < 55; i++) {
			for (int j = 0; j < 55; j++) {
				for (int k = 0; k < 55; k++) {
					for (int p = 0; p < 55; p++) {
						f[i][j][k][p] = -1;
					}
				}
			}
		}
		System.out.println(dfs(1, 1, n, n));
 
	}
 
	static int dfs(int x, int y, int z, int w) {
		if (f[x][y][z][w] != -1)
			return f[x][y][z][w];
		if (x == z && y == w)
			return f[x][y][z][w] = (s[x].charAt(y) == '#' ? 1 : 0);
		int ret = Math.max(z - x + 1, w - y + 1);
		for (int i = x; i < z; i++)
			ret = Math.min(dfs(x, y, i, w) + dfs(i + 1, y, z, w), ret);
		for (int i = y; i < w; i++)
			ret = Math.min(dfs(x, y, z, i) + dfs(x, i + 1, z, w), ret);
		return f[x][y][z][w] = ret;
	}
 
	static int nextNum() {
		try {
			st.nextToken();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return (int) st.nval;
	}
 
	static int log2(long x) {
		int flag = 1;
		int log = 0;
		while (x > flag) {
			flag <<= 1;
			log++;
		}
		return log;
	}
 
	static long phi(int x) {
		long res = x;
		for (long i = 2; i <= Math.sqrt(x); i++) {
			if (x % i == 0) {
				while (x % i == 0)
					x /= i;
				res = res / i * (i - 1);
			}
		}
		if (x > 1)
			res = res / x * (x - 1);
		return res;
	}
 
	static long pow(long a, long b, long mod) {
		if (b == 0)
			return 1;
		if (b == 1)
			return a;
		if ((b & 1) == 0)
			return pow(a * a % mod, b >> 1, mod);
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
 
class Node {
	int a;
	int b;
	int c;
 
	public Node() {
	}
 
	public Node(int a, int b, int c) {
		super();
		this.a = a;
		this.b = b;
		this.c = c;
	}
 
	@Override
	public String toString() {
		return "Node [a=" + a + ", b=" + b + "]";
	}
}
```

