---
title: 'Codeforces Round #573 Tokitsukaze and Discard Items'
date: 2019-07-13 14:29:27
tags: 
 - 模拟
 - Java
categories:
 - 模拟
---
## A. Tokitsukaze and Discard Items
> time limit per test1 second
memory limit per test256 megabytes
inputstandard input
outputstandard output
Recently, Tokitsukaze found an interesting game. Tokitsukaze had n items at the beginning of this game. However, she thought there were too many items, so now she wants to discard m (1≤m≤n) special items of them.

These n items are marked with indices from 1 to n. In the beginning, the item with index i is placed on the i-th position. Items are divided into several pages orderly, such that each page contains exactly k positions and the last positions on the last page may be left empty.

Tokitsukaze would do the following operation: focus on the first special page that contains at least one special item, and at one time, Tokitsukaze would discard all special items on this page. After an item is discarded or moved, its old position would be empty, and then the item below it, if exists, would move up to this empty position. The movement may bring many items forward and even into previous pages, so Tokitsukaze would keep waiting until all the items stop moving, and then do the operation (i.e. check the special page and discard the special items) repeatedly until there is no item need to be discarded.
![操作描述](https://raw.githubusercontent.com/xfx98/ms/master/img/cf-TokitsukazeandDiscardItems.png)
Consider the first example from the statement: n=10, m=4, k=5, p=[3,5,7,10]. The are two pages. Initially, the first page is special (since it is the first page containing a special item). So Tokitsukaze discards the special items with indices 3 and 5. After, the first page remains to be special. It contains [1,2,4,6,7], Tokitsukaze discards the special item with index 7. After, the second page is special (since it is the first page containing a special item). It contains [9,10], Tokitsukaze discards the special item with index 10.
Tokitsukaze wants to know the number of operations she would do in total.

## Input
The first line contains three integers n, m and k (1≤n≤1018, 1≤m≤105, 1≤m,k≤n) — the number of items, the number of special items to be discarded and the number of positions in each page.

The second line contains m distinct integers p1,p2,…,pm (1≤p1<p2<…<pm≤n) — the indices of special items which should be discarded.

## Output
Print a single integer — the number of operations that Tokitsukaze would do in total.

## Examples
input
>10 4 5
3 5 7 10

output
>3

input

>13 4 5
7 8 9 10

output
>1
## Note
>For the first example:
In the first operation, Tokitsukaze would focus on the first page [1,2,3,4,5] and discard items with indices 3 and 5;
In the second operation, Tokitsukaze would focus on the first page [1,2,4,6,7] and discard item with index 7;
In the third operation, Tokitsukaze would focus on the second page [9,10] and discard item with index 10.
For the second example, Tokitsukaze would focus on the second page [6,7,8,9,10] and discard all special items at once.

这题的意思是在总共有n个数，每页有k个数，删除m个指定的数，每页删除的任意多个为1次操作，删除之后，后面的数将向前补齐，问总共需要几次操作。
这题是一个模拟题，每次从前删除桶一个页面的数，用id记录已删除的个数，然后用`(arr[i + j] - id) / k == (arr[i] - id) / k)`判断是否在同一页，记录操作数即可。小心数据范围和数组越界这里在数组中加一个`arr[m]= -k-1`来防止数组越界，因为越界问题re了四发（哭）。
下面是java的AC代码
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
import java.util.Scanner;
 
public class Main {
	static StreamTokenizer st = new StreamTokenizer(new BufferedInputStream(System.in));
	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static PrintWriter pr = new PrintWriter(new BufferedOutputStream(System.out));
	static Scanner sc = new Scanner(System.in);
 
	public static void main(String[] args) throws NumberFormatException, IOException {
		long n = sc.nextLong();
		int m = sc.nextInt();
		long k = sc.nextLong();
		long arr[] = new long[m + 1];
		for (int i = 0; i < m; i++) {
			arr[i] = sc.nextLong();
		}
		arr[m]= -k-1;
		long id = 1;
		int cont = 0;
		for (int i = 0; i < m;) {
			int j = 1;
			while (((arr[i + j] - id) / k == (arr[i] - id) / k))
				j++;
			i += j;
			id = (id + j) % k;
			cont++;
		}
		System.out.println(cont);
	}
 
	private static int nextInt() {
		try {
			st.nextToken();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return (int) st.nval;
	}
}
```


