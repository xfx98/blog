---
title: 'Codeforces Round #573 Tokitsukaze, CSL and Stone Game'
date: 2019-07-13 14:34:27
tags: 
 - 博弈
 - Java
categories:
 - 博弈
---
## B. Tokitsukaze, CSL and Stone Game
>time limit per test1 second
memory limit per test256 megabytes
inputstandard input
outputstandard output
Tokitsukaze and CSL are playing a little game of stones.

In the beginning, there are n piles of stones, the i-th pile of which has ai stones. The two players take turns making moves. Tokitsukaze moves first. On each turn the player chooses a nonempty pile and removes exactly one stone from the pile. A player loses if all of the piles are empty before his turn, or if after removing the stone, two piles (possibly empty) contain the same number of stones. Supposing that both players play optimally, who will win the game?

Consider an example: n=3 and sizes of piles are a1=2, a2=3, a3=0. It is impossible to choose the empty pile, so Tokitsukaze has two choices: the first and the second piles. If she chooses the first pile then the state will be [1,3,0] and it is a good move. But if she chooses the second pile then the state will be [2,2,0] and she immediately loses. So the only good move for her is to choose the first pile.

Supposing that both players always take their best moves and never make mistakes, who will win the game?

Note that even if there are two piles with the same number of stones at the beginning, Tokitsukaze may still be able to make a valid first move. It is only necessary that there are no two piles with the same number of stones after she moves.

## Input
The first line contains a single integer n (1≤n≤105) — the number of piles.

The second line contains n integers a1,a2,…,an (0≤a1,a2,…,an≤109), which mean the i-th pile has ai stones.

## Output
Print "sjfnb" (without quotes) if Tokitsukaze will win, or "cslnb" (without quotes) if CSL will win. Note the output characters are case-sensitive.

## Examples
input
>0
output
cslnb

input
>2
1 0

output
>cslnb

input
>2
2 2

output
>sjfnb

input
>3
2 3 1

output
>sjfnb

## Note
>In the first example, Tokitsukaze cannot take any stone, so CSL will win.
In the second example, Tokitsukaze can only take a stone from the first pile, and then, even though they have no stone, these two piles will have the same number of stones, which implies CSL will win.
In the third example, Tokitsukaze will win. Here is one of the optimal ways:
Firstly, Tokitsukaze can choose the first pile and take a stone from that pile.
Then, CSL can only choose the first pile, because if he chooses the second pile, he will lose immediately.
Finally, Tokitsukaze can choose the second pile, and then CSL will have no choice but to lose.
In the fourth example, they only have one good choice at any time, so Tokitsukaze can make the game lasting as long as possible and finally win.

这题是一个博弈游戏，有n堆石头，两人接连操作，每次可以移去一堆中的一个，如果操作后出现一次有两个相同数量的堆，则输。当初始有两堆相同时，如果能够在操作之后没有相同的，游戏可以继续进行。
首先出现开局必败的局面有：

 - 三个相同的
 - 两对及以上相同的
 - 两个0
 - 一对相同的，但这对相同的中，比它小1的数存在
 - 总和小于`n*(n-1)/2`，这种情况出现必会出现上述三种情况。
 
 以上情况无法通过一次操作而不出现相同堆，所以初始必败态。
 除必败态意外，操作的终态一定会是 `0 1 2 3....n-1`这是最后不能操作的形式，任意操作都会出现两个相同堆。因为每次只能操作一对中的一个，而在终态之前，总能找到合适的操作。这样可操作的数量便是`sum - n*(n-1)/2`然后分奇偶即可的出谁是赢家。
以下是java的AC代码

```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.io.StreamTokenizer;
import java.math.BigInteger;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.Scanner;
 
public class Main {
	static StreamTokenizer st = new StreamTokenizer(new BufferedInputStream(System.in));
	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static PrintWriter pr = new PrintWriter(new BufferedOutputStream(System.out));
	static Scanner sc = new Scanner(System.in);
 
	public static void main(String[] args) throws NumberFormatException, IOException {
		long sum = 0;
		long n = sc.nextLong();
		HashSet<Long> hs = new HashSet<>();
		ArrayList<Long> al = new ArrayList<>();
		for (int i = 0; i < n; i++) {
			long te = sc.nextLong();
			sum += te;
			if(!hs.add(te)) {
				al.add(te);
			}
		}
		for (Long ye : al) {
			if(hs.contains(ye-1)||ye==0) {
				System.out.println("cslnb");
				System.exit(0);
			}
		}
		if (hs.size() < n - 1) {
			System.out.println("cslnb");
		} else {
			sum-=n*(n-1)/2;
			if (sum < 0) {
				System.out.println("cslnb");
			} else if ((sum & 1) == 0) {
				System.out.println("cslnb");
			} else {
				System.out.println("sjfnb");
			}
		}
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

