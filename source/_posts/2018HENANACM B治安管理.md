---
title: '2018HENANACM B治安管理'
date: 2018-06-20 20:43:17
tags:
 - Java
 - 差分区间
categories:
 - 差分区间
---
题目描述
    SZ市是中国改革开放建立的经济特区，是中国改革开放的窗口，已发展为有一定影响力的国际化城市，创造了举世瞩目的“SZ速度”。SZ市海、陆、空、铁口岸俱全，是中国拥有口岸数量最多、出入境人员最多、车流量最大的口岸城市. 
    为了维护SZ经济特区社会治安秩序，保障特区改革开放和经济建设的顺利进行, 特别设立了SZ社会治安综合治理委员会主管特区的社会治安综合治理工作。公安机关是社会治安的主管部门，依照法律、法规的规定进行治安行政管理，打击扰乱社会治安的违法犯罪行为，维护社会秩序。
    YYH大型活动将在[S, F)这段时间举行，现要求活动期间任何时刻巡逻的警察人数不少于M人。公安机关将有N名警察在维护活动的安全，每人巡逻时间为[ai, bi)。请你检查目前的值班安排，是否符合要求。若满足要求，输出YES，并输出某个时刻同时巡逻的最多人数；若不满足要求，输出NO，并输出某个时刻同时巡逻的最少人数。


输入
第一行： T   表示以下有T组测试数据（ 1≤ T ≤5 ）
对每组数据，   
第一行：N  M  S  F        ( 1≤N≤10000  1≤M ≤1000  0≤S<F≤100000）
第二行，a1  a2 ….  an     警察巡逻起始时间  
第三行，b1  b2 ….  bn     警察巡逻结束时间     (  0≤ ai < bi ≤100000  i=1…. n）

输出
对每组测试数据，输出占一行。若满足要求，输出YES，并输出某个时刻同时巡逻的最多人数；若不满足要求，输出NO，并输出某个时刻同时巡逻的最少人数。（中间一个空格）
样例输入
>2
5 2 0 10
0 0 2 7 6
6 2 7 10 10
10 2 6 11
1 3 5 7 9 2 4 6 8 10
2 4 6 8 10 3 5 7 9 11

样例输出
>YES 2
NO 1

差分区间求覆盖情况，然后得出最大最小值就行
java AC 代码
```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int t = sc.nextInt();
		while(t-->0) {
			int n = sc.nextInt();
			int m = sc.nextInt();
			int s = sc.nextInt();
			int f = sc.nextInt();
			int[] data = new int[100005];
			for (int i = 0; i < n; i++) {
				data[sc.nextInt()]++;
			}
			for (int i = 0; i < n; i++) {
				data[sc.nextInt()]--;
			}
			int sum = 0;
			for (int i = 0; i <= f; i++) {
				sum += data[i];
				data[i]=sum;
			}
			int max = data[s];
			int min = data[s];
			for(int i = s;i<f;i++) {
				if(data[i]>max) {
					max = data[i];
				}
				if(data[i]<min) {
					min = data[i];
				}
			}
			if(min>=m) {
				System.out.println("YES "+max);
			}else {
				System.out.println("NO "+min);
			}
		}
	}
}

```

