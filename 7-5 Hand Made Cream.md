# 7-5 Hand Made Cream

### Introduction:

Suppose you are a baker planning to bake some hand-made cream breads.

To bake a cream bread, we need to use one slice of bread and one kind of cream. Each hand-made cream bread has a taste score to describe how delicious it is, which is obtained by multiplying the taste score for bread and the taste score for cream. (The taste scores could be negative, however, two negative taste scores can still produce delicious cream bread.)

The bread and cream are stored separately.

The constraint is that you need to examine the breads in a given order, and for each piece of bread, you have to decide immediately (without looking at the remaining breads) whether you would like to take it.

After you are finished with breads, you will take the same amount of cream in the same manner. The breads and creams you take will be paired in the same order as you take them.

Given *N* taste scores for bread and *M* taste scores for cream, you are supposed to calculate the maximum total taste scores for all hand-made cream bread.

### Input Specification:

Each input file contains one test case. For each case, the first line contains two integers *N* and *M* (1≤*N*,*M*≤10^3), which are the numbers of bread and cream, respectively.

The second line gives *N* taste scores for bread.

The third line gives *M* taste scores for cream.

The taste scores are integers in [−10^3,10^3].

All the numbers in a line are separated by a space.

### Output Specification:

Print in a line the maximum total taste score.

### Sample Input:

```in
3 4
-1 10 8
10 8 11 9
```

### Sample Output:

```out
188
```

### Hint:

The maximum total taste score for the sample case is 10×10+8×11=188.

![hint.png](C:\Users\zjc\Desktop\39dabd46-4c84-4862-b386-9d38664b86c9.png)

### 解题思路：

本题考查动态规划

思路：考察第一个面包`bread[1]`，选到第`i`个面包`bread[i]`和从`cream[1]`到`cream[j]`的乘积最大结果，我们分为匹配和不匹配两种情况讨论。

如果将第`i`个面包和第`j`个奶油搭配到一起，那么我们的二维数组`match[i][j] = match[i-1][j-1] + bread[i] * cream[j]; `

如果第`i`个面包和地`j`个奶油并不搭配，那么等于前一个面包和当前奶油的乘积或者前一个奶油和当前面包的乘积两者的最大值，即`match[i][j] = max(match[i-1][j],match[i][j-1]);`

### 题解代码：

```C++
#include <bits/stdc++.h>
using namespace std;
int N, M;
int bread[1001], cream[1001];
int match[1001][1001];

int max(int a, int b) {
	if (a > b) 
		return a;
	else
		return b;
}

int main() {
	cin >> N >> M;
	for (int i = 1; i <= N; i++) {
		cin >> bread[i];
	}
	for (int j = 1; j <= M; j++) {
		cin >> cream[j];
	}
	//i和j配对就是match[i][j] = match[i-1][j-1]+bread[i]*cream[j]
	//不配对就是match[i][j] = max(match[i-1][j],match[i][j-1])
	int sum = 0;
	for (int i = 0; i <= N; i++) {
		for (int j = 0; j <= M; j++) {
			match[i][j] = 0;
		}
	}
	for (int i = 1; i <= N; i++) {
		for (int j = 1; j <= M; j++) {
			int m = match[i - 1][j - 1] + bread[i] * cream[j];
			int n = max(match[i - 1][j], match[i][j - 1]);
			if (m >= n) {
				match[i][j] = m;
			}
			else {
				match[i][j] = n;
			}
			sum = match[i][j];
		}
	}
	cout << sum << endl;
	//选取的元素进行相乘时有一个原则，m>=n
	return 0;
}
```

