---
title: 'Codeforces Round #576 (Div. 1)  E. Rectangle Painting 2'
date: 2019-09-7 21:49:12
tags: 
 - 网络流
 - 二分图匹配
 - Java
categories:
 - 网络流
---
## Rectangle Painting 2
There is a square grid of size n×n. Some cells are colored in black, all others are colored in white. In one operation you can select some rectangle and color all its cells in white. It costs min(h,w) to color a rectangle of size h×w. You are to make all cells white for minimum total cost.

The square is large, so we give it to you in a compressed way. The set of black cells is the union of m rectangles.

## Input
The first line contains two integers n and m (1≤n≤109, 0≤m≤50) — the size of the square grid and the number of black rectangles.

Each of the next m lines contains 4 integers xi1 yi1 xi2 yi2 (1≤xi1≤xi2≤n, 1≤yi1≤yi2≤n) — the coordinates of the bottom-left and the top-right corner cells of the i-th black rectangle.

The rectangles may intersect.

## Output
Print a single integer — the minimum total cost of painting the whole square in white.

## Examples
input
>10 2
4 1 5 10
1 4 10 5

output
>4

input
>7 6
2 1 2 1
4 2 4 3
2 5 2 5
2 3 5 3
1 2 1 2
3 2 5 3

output
>3

该题的意思是，在一个n*n正方形的图中，有m个矩形的方块是黑色的，其它的是白色的。而使一个a*b的矩形变成白色需要min(a,b)的代价。从这里可以看出可以每次使整行或者整列变成白色和不疯白色是一样的代价。
这样就是一个最小点覆盖的问题。最小点覆盖=二分图中的最大网络流。点是行和列。边是图中的黑色色块。但由于n的数可能很大，导致图很大，直接用二分图肯定不行的。需要进行离散化。将一样的行和列进行合并，组成新的容量。
离散之后建立源点和汇点，分别和行列相连，容量就是合并之后形成的容量，行和列相连组成的边容量无穷大。图建好之后跑最大流算法得出结果。

做的第一个网络流怎么也得留个纪念。。。。

```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.io.StreamTokenizer;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class Main {
	static StreamTokenizer st = new StreamTokenizer(new BufferedInputStream(System.in));
	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static PrintWriter pr = new PrintWriter(new BufferedOutputStream(System.out));
	static Scanner sc = new Scanner(System.in);
//		long tic = System.currentTimeMilis();
//		long toc = System.currentTimeMillis();
//		System.out.println("Elapsed time: " + (toc - tic) + " ms");
	/**
	 * 二分图匹配->最小定点覆盖
	 * a[]、b[] 分别代表离散化的行和列 c[]代表离散简化后的图。
	 * x1[]、x2[]、y1[]、y2[] ,代表对应的黑色方块。
	 * ss源点 tt汇点
	 * e[]代表二分图中的边,链式前向星 存图，h[]表示该点前一条边的位置,Node.v是该点对应的另一个，节点 Node.f代表容量，Node.nx下一条边
	 * 
	 */
	static Node e[] = new Node[10000001];
	static int et = 1, h[] = new int[10001], d[] = new int[10001], ss, tt;
	static int a[] = new int[101], b[] = new int[101], c[][] = new int[101][101];
	static int x1[] = new int[101], y1[] = new int[101], x2[] = new int[101], y2[] = new int[101];

	public static void main(String[] args) {
		int n = nextInt(), m = nextInt();
		for (int i = 1; i <= m; i++) {
			x1[i] = nextInt();
			y1[i] = nextInt();
			x2[i] = nextInt();
			y2[i] = nextInt();
			a[++a[0]] = x1[i];
			a[++a[0]] = x2[i] + 1;
			b[++b[0]] = y1[i];
			b[++b[0]] = y2[i] + 1;
		}
		Arrays.sort(a, 1, a[0] + 1);
		a[0] = unique(a, 1, a[0] + 1);
 
		Arrays.sort(b, 1, b[0] + 1);
		b[0] = unique(b, 1, b[0] + 1) ;
		for (int i = 1; i <= m; i++) {
			x1[i] = lower_bound(a, 1, a[0] + 1, x1[i]);
			x2[i] = lower_bound(a, 1, a[0] + 1, x2[i] + 1) - 1;
			y1[i] = lower_bound(b, 1, b[0] + 1, y1[i]);
			y2[i] = lower_bound(b, 1, b[0] + 1, y2[i] + 1) - 1;
			for (int u = x1[i]; u <= x2[i]; u++)
				for (int v = y1[i]; v <= y2[i]; v++)
					c[u][v] = 1;
		}
		ss = 0;
		tt = a[0] + b[0] + 1;
		for (int i = 1; i <= a[0]; i++)
			for (int j = 1; j <= b[0]; j++)
				if (c[i][j] != 0)
					jb(i, a[0] + j, (int) 2e9);
		for (int i = 1; i < a[0]; i++)
			jb(ss, i, a[i + 1] - a[i]);
		for (int i = 1; i < b[0]; i++)
			jb(a[0] + i, tt, b[i + 1] - b[i]);
		int ans = 0;
		while (bfs()) {
			ans += dfs(ss, (int) 2e9);
		}
		System.out.printf("%d\n", ans);
	}
 
	private static int lower_bound(int[] arr, int l, int r, int it) {
		int m = (l+r)>>1;
		while(l<=r) {
			if(it > arr[m]) {
				l = m+1;
				m = (l+r)>>1;
			}else if(it<arr[m]) {
				r = m-1;
				m = (l+r)>>1;
			}else {
				return m;
			}
		}
		return m;
	}
 
	private static int unique(int[] a2, int a, int b) {
		int te = a;
		for (int i = a + 1; i < b; i++) {
			if (a2[i] != a2[te]) {
				a2[++te] = a2[i];
			}
		}
		return te - a + 1;
	}
 
	static void jb(int u, int v, int f) {
		e[++et] = new Node(v, f, h[u]);
		h[u] = et;
		e[++et] = new Node(u, 0, h[v]);
		h[v] = et;
	}
 
	static boolean bfs() {
		for (int i = ss; i <= tt; i++) {
			d[i] = 0;
		}
		Queue<Integer> q = new LinkedList<Integer>();
		q.add(ss);
		d[ss] = 1;
		while (!q.isEmpty()) {
			int u = q.poll();
			for (int i = h[u]; i != 0; i = e[i].nx)
				if (e[i].f != 0 && d[e[i].v] == 0) {
					d[e[i].v] = d[u] + 1;
					q.add(e[i].v);
				}
		}
		return d[tt] != 0;
	}
 
	static int dfs(int u, int f) {
		if (f == 0 || u == tt)
			return f;
		int F = 0;
		for (int i = h[u]; i != 0; i = e[i].nx)
			if (d[e[i].v] == d[u] + 1) {
				int ff = dfs(e[i].v, Math.min(e[i].f, f));
				e[i].f -= ff;
				e[i ^ 1].f += ff;
				f -= ff;
				F += ff;
				if (f == 0)
					break;
			}
		return F;
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
class Node {
	int v, f, nx;

	public Node() {
	}

	public Node(int v, int f, int nx) {
		this.v = v;
		this.f = f;
		this.nx = nx;
	}
}

```

