---
title: 'poj3278,cow'
date: 2018-09-18 21:55:32
tags:
 - bfs
 - Java
categories:
 - bfs
---
[题目链接](http://poj.org/problem?id=3278)
## Catch That Cow
Time Limit: 2000MS		Memory Limit: 65536K
Total Submissions: 120702		Accepted: 37665
Description

Farmer John has been informed of the location of a fugitive cow and wants to catch her immediately. He starts at a point N (0 ≤ N ≤ 100,000) on a number line and the cow is at a point K (0 ≤ K ≤ 100,000) on the same number line. Farmer John has two modes of transportation: walking and teleporting.

* Walking: FJ can move from any point X to the points X - 1 or X + 1 in a single minute
* Teleporting: FJ can move from any point X to the point 2 × X in a single minute.

If the cow, unaware of its pursuit, does not move at all, how long does it take for Farmer John to retrieve it?

Input

Line 1: Two space-separated integers: N and K
Output

Line 1: The least amount of time, in minutes, it takes for Farmer John to catch the fugitive cow.
Sample Input



>5 17





Sample Output

>4


## Hint

>The fastest way for Farmer John to reach the fugitive cow is to move along the following path: 5-10-9-18-17, which takes 4 minutes.


<hr>


该题为搜索题，只需将当前位置的走法遍历，直到找到终点，采用bfs效率较高、
搜索入门题（
ac代码
```java

import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class Main {
	public static void main(String args[]) {
		Scanner sc = new Scanner(System.in);
		Queue<Integer> d = new LinkedList<Integer>();
		boolean bo[] = new boolean[100001];
		int n = sc.nextInt();
		int k = sc.nextInt();
		d.offer(n);
		int count = 0;
		int num = 1;
		if (k <= n) {
			System.out.println(n - k);
			System.exit(0);
		}
		while (!d.isEmpty()) {
			int ad = 0;
			count++;
			for (int i = 0; i < num; i++) {
				int stp = d.poll();
				int next = stp - 1;
				if (next >= 0 && !bo[next]) {
					bo[next] = true;
					d.offer(next);
					ad++;
					if (next == k) {
						System.out.println(count);
						System.exit(0);
					}
				}

				next = stp + 1;
				if (next < 100001 && !bo[next]) {
					bo[next] = true;
					d.offer(next);
					ad++;
					if (next == k) {
						System.out.println(count);
						System.exit(0);
					}
				}

				next = stp << 1;
				if (next < 100001 && !bo[next]) {

					bo[next] = true;
					d.offer(next);
					ad++;
					if (next == k) {
						System.out.println(count);
						System.exit(0);
					}
				}
			}

			num = ad;
		}

	}

}


```