---
title: 'Educational Codeforces Round 68 D.1-2-K Game'
date: 2019-07-15 1:08:27
tags: 
 - 博弈
 - Java
categories:
 - 博弈
---
## D. 1-2-K Game
>time limit per test2 seconds
memory limit per test256 megabytes
inputstandard input
outputstandard output
Alice and Bob play a game. There is a paper strip which is divided into n + 1 cells numbered from left to right starting from 0. There is a chip placed in the n-th cell (the last one).

Players take turns, Alice is first. Each player during his or her turn has to move the chip 1, 2 or k cells to the left (so, if the chip is currently in the cell i, the player can move it into cell i - 1, i - 2 or i - k). The chip should not leave the borders of the paper strip: it is impossible, for example, to move it k cells to the left if the current cell has number i < k. The player who can't make a move loses the game.

Who wins if both participants play optimally?

Alice and Bob would like to play several games, so you should determine the winner in each game.

## Input
The first line contains the single integer T (1 ≤ T ≤ 100) — the number of games. Next T lines contain one game per line. All games are independent.

Each of the next T lines contains two integers n and k (0 ≤ n ≤ 109, 3 ≤ k ≤ 109) — the length of the strip and the constant denoting the third move, respectively.

## Output
For each game, print Alice if Alice wins this game and Bob otherwise.

## Example
input
>4
0 3
3 3
3 4
4 4

output
>Bob
Alice
Bob
Alice

这题是一个博弈论题，意思大概为n米长，每次可以移动1，2，k米不能移动的人输。这题可以用sg函数进行打表得出规律。最后能够得出当`k%3=0`时会对原博弈结果产生影响。否则就是和只有能移动1，2米的结果相同，最终结果可以通过`%k+1`进行分类，能%3的k影响结果只是在k是能影响，其它的仍然可以通过%3来进行结果的判断。
AC代码java

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
		int arr[] = new int[10000];
		int q = sc.nextInt();
		while (q-- > 0) {
			int n = sc.nextInt();
			int k = sc.nextInt();
			if(k%3==0) {
				n %= k+1;
				if(n==k) {
					System.out.println("Alice");
				}else {
					if(n%3==0) {
						System.out.println("Bob");					
					}else {
						System.out.println("Alice");
					}
				}
			}else {
				if(n%3==0) {
					System.out.println("Bob");					
				}else {
					System.out.println("Alice");
				}
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

