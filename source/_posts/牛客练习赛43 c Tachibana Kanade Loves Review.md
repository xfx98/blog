---
title: '牛客练习赛43 c题 Tachibana Kanade Loves Review'
date: 2019-04-07 21:45:17
tags:
 - Java
 - 差分区间
categories:
 - 差分区间
---
[题目链接](https://ac.nowcoder.com/acm/contest/548/C)


#### 题目描述
立华奏是一个刚刚开始学习 OI 的萌新。
最近，实力强大的 Qingyu 当选了 IODS 9102 的出题人。众所周知， IODS 是一场极其毒瘤的比赛。为了在这次比赛中取得好的成绩，立华奏决定学习可能考到的每一个知识点。
在 Qingyu 的博客中，立华奏得知这场比赛总共会考察选手 n 个知识点。此前，立华奏已经依靠自学学习了其中 k 个知识点。接下来，立华奏需要学习其他的知识点，每学习一个单独的知识点，需要消耗的时间为 Ti 天。同时，某些知识点之间存在联系，可以加速学习的过程。经过计算，立华奏一共发现了其中 m 种联系，第 i 种联系可以表示为(Xi,Yi,Hi)，其含义为“在掌握了第 Xi 个知识点和第 Yi 个知识点中任意一个后，学习 Hi 天即可掌握另一个知识点”。
留给立华奏的时间所剩无几，只有 t 天，因此，她想知道自己能不能在这 t 天内学习完成所有的知识点。
#### 输入描述:
本题输入量较大，请注意使用效率较高的读入方式
输入的第一行包含四个整数 n, m, k, t，含义见上所述。
接下来一行，包含 n 个整数，依次表示 T1,T2,?,Tn
接下来一行，包含 k 个整数，表示立华奏已经学习过的知识点。如果 k=0，则此处为一空行。
接下来 m 行，每行 3 个整数 Xi,Yi,Hi，描述一种联系。
#### 输出描述:
如果立华奏能够学习完所有的知识点，输出一行 Yes。否则输出 No

#### 示例1
#### 输入
>4 3 2 5
4 5 6 7
2 3
1 2 3
1 3 2
3 4 2
#### 输出
>Yes
##### 说明
>立华奏已经学习过了第 2, 3 个知识，由第 2 个关系，立华奏可以花 2 天学会知识点 1，在由关系 3， 立华奏可以 2 天学会知识点 4，因此总共需要花费 4 天，可以完成任务。
#### 示例2
#### 输入
>5 4 0 12
4 5 6 7 1
&nbsp;
1 2 3
1 3 2
3 4 2
1 5 233
#### 输出
>Yes
#### 说明
>立华奏比较菜，因此什么都没有学过。她可以选择先花 4 天的时间学会知识点 1。然后根据关系 1, 2，分别花 3, 2 天的时间学会知识点 2, 3，再根据关系 3，花 2 天的时间学会知识点 4。然后，她再单独学习知识点 5，花费1天，总共花费了 12 天 ，可以完成任务。
请注意，虽然关系 4 允许立华奏在知识点 1 的基础上学习知识点 5，但需要的时间比单独学习还要多，因此立华奏不会在知识点 1 的基础上学习知识点 5.
#### 备注:
>0?k?n?1e6,m?5e6,t?1e18,Ti,Hi?1e3

可以转换为最小生成树问题。

java因超时没能过。。。可能是读的有点慢吧。。。
目前这是我能把常数减到最小的了。。。

```java
import java.io.BufferedInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.StreamTokenizer;
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.PriorityQueue;
import java.util.Scanner;

public class Main {

	static StreamTokenizer st = new StreamTokenizer(new BufferedInputStream(System.in));

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = nextNum();
		int m = nextNum();
		int k = nextNum();
		long t = nextNum();
		int T[] = new int[n + 1];
		PriorityQueue<Node> pq = new PriorityQueue<>();
		for (int i = 1; i <= n; i++) {
			T[i] = nextNum();
			pq.add(new Node(0, i, T[i], 0));
		}
		boolean has[] = new boolean[n + 1];
		boolean use[] = new boolean[m + 1];
		has[0] = true;
		int K[] = new int[k];
		for (int i = 0; i < k; i++) {
			K[i] = nextNum();
			has[K[i]] = true;
		}

		ArrayList<ArrayList<Node>> al = new ArrayList<>();
		for (int i = 0; i <= n; i++) {
			al.add(new ArrayList<>());
		}
		int flag = k;
		for (int i = 1; i <= m; i++) {
			Node node = new Node(nextNum(), nextNum(), nextNum(), i);
			al.get(node.x).add(node);
			al.get(node.y).add(node);
		}

		for (int i = 0; i < k; i++) {
			for (Node node : al.get(K[i])) {
				if (!use[node.id]) {
					pq.add(node);
					use[node.id] = true;
				}
			}
		}

		long ans = 0;
		while (!pq.isEmpty()) {
			Node te = pq.poll();
			if (has[te.x] && has[te.y]) {
				continue;
			} else if (has[te.x]) {
				flag++;
				ans += te.h;
				has[te.y] = true;
				for (Node node : al.get(te.y)) {
					if ((has[node.x] && has[node.y]) || use[node.id]) {
						continue;
					} else {
						pq.add(node);
						if(node.id!=0) {
							use[node.id] = true;
						}
					}
				}
			} else {
				flag++;
				ans += te.h;
				has[te.x] = true;
				for (Node node : al.get(te.x)) {
					if ((has[node.x] && has[node.y]) || use[node.id]) {
						continue;
					} else {
						pq.add(node);
						if(node.id!=0) {
							use[node.id] = true;
						}
					}
				}
			}
			if (flag >= n) {
				break;
			}
			if (ans > t) {
				System.out.println("No");
				return;
			}
		}
		System.out.println("Yes");
		return;
	}
	private static int nextNum() {
		try {
			st.nextToken();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return (int) st.nval;

	}
}

class Node implements Comparable<Node> {
	int x;
	int y;
	int h;
	int id;

	public Node(int x, int y, int h, int id) {
		super();
		this.x = x;
		this.y = y;
		this.h = h;
		this.id = id;
	}

	@Override
	public String toString() {
		return "Node [x=" + x + ", y=" + y + ", h=" + h + "]";
	}

	@Override
	public int compareTo(Node o) {

		return h - o.h;
	}

}
```

