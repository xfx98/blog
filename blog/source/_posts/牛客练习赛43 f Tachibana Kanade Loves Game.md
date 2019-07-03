---
title: '牛客练习赛34 c题 little w and Segment Coverage'
date: 2019-04-07 21:43:17
tags:
 - Java
 - 容斥原理
categories:
 - 容斥原理
---
[题目链接](https://ac.nowcoder.com/acm/contest/548/F)

#### 题目描述 

立华奏是一个天天打比赛的萌新。

省选将至，萌新立华奏深知自己没有希望进入省队，因此开始颓废。她正在颓废一款名为《IODS 9102》的游戏。

在游戏中，立华奏拥有 k 点血量，而她的对手拥有 q 点血量。当她的血量变为 0 时，游戏便结束了；同理，如果对方的血量变为 0，立华奏就获胜了。在立华奏手中，有 n 种武器，编号分别为1,2,⋯,n，每一种武器在使用后，都能让对方受到 1 点伤害，且此后不得再次使用这个武器。同时，对方拥有m−1 种反击魔咒，编号分别为 2,3,4,⋯,m（如果 m = 1，则可认为此时不具有反击魔咒）。如果立华奏在使用第 i 种武器攻击对方时，对方恰好有编号为 j 的魔咒，且j∣i， 那么立华奏会受到 1 点伤害（注意此时，攻击仍然是有效的，即对方的血量仍然会减少 1），同时对方也可以再次使用这个反击魔咒。
由于立华奏是个萌新，因此对方保证不会主动攻击立华奏 。
现在，立华奏想要知道，自己是否存在一种攻击方案，使得自己取得胜利。

#### 输入描述:

> 输入包含多组数据。
输入的第一行包含一个整数 T，表示数据组数。
接下来 T 行，每行包含四个整数 k, q, n, m，描述一组数据。

#### 输出描述:
>输出 T 行，每行描述一组数据的解。如果本组数据中，立华奏存在必胜策略，则输出 Yes，否则输出 QAQ。
你可以认为数据保证不会出现平局的情形。

#### 示例1
#### 输入
>5
0 23333 2333333 5
1 1999999999 29999999999999 9
1 998244353998244 12345678 9
1 3 3 4
1 5 6 7

#### 输出
>QAQ
Yes
QAQ
QAQ
QAQ

#### 说明
>对于第一组样例，立华奏开始就死掉了，因此答案为QAQ

>对于第二组样例，你只需要使用所有的不含{2,3,4,5,6,7,8,9}因子的武器即可，显然在 29999999999999 内存在这些武器
对于第三组样例，立华奏的武器只有12345678个，但她的对手血量更多，显然她不可能取胜
对于第四组样例，你的血量为1，代表你不能使用会触发反击魔咒的武器，答案为QAQ
对于第五组样例，与第四组样例是相同的
#### 备注:
>1⩽T⩽1e5,0⩽k⩽1e18,0<q⩽1e18,0⩽n⩽1e18,1⩽m⩽20

题目是要求出 1-n 不能整除 2-m 的数，很显然可以用容斥，用2-m的质数的容斥
以下java代码有可能一次无法通过。可能因为读取还是慢。如果有更快的读写，请大佬能抬一手QAQ

```java
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.StreamTokenizer;

public class Main {
	static StreamTokenizer st = new StreamTokenizer(new InputStreamReader(System.in));

	public static void main(String[] args) {
		int t = (int) nextNum();
		int arr[] = { 2, 3, 5, 7, 11, 13, 17, 19 };
		while (t-- > 0) {
			long k = nextNum();
			long q = nextNum();
			long n = nextNum();
			long m = nextNum();
			int tot = 0;
			for (int i = 0; i < 8; i++) {
				if (m >= arr[i])
					++tot;
				else
					break;
			}
			long ans = 0;
			for (int i = 1; i < (1 << tot); i++) { // 枚举素数 // 所有的组合//有i二进制表示 1表示使用到该数，0表示表示未使用、这样就能枚举出所有组合
				long temp = 1;
				int cnt = 0;
				for (int j = 0; j < tot; j++)
					if ((i & (1 << j)) != 0) {
						cnt++;
						temp *= arr[j];
					}
				if ((cnt & 1) != 0)
					ans += n / temp; // 奇加偶减
				else
					ans -= n / temp;
			}
			ans = n - ans;
			if (k != 0 && ans + k > q) {
				System.out.println("Yes");
			} else {
				System.out.println("QAQ");
			}
		}
	}

	private static long nextNum() {
		try {
			st.nextToken();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return (long) st.nval;

	}
}

```

