---
description: 注意、Amazon
---

# Modern Ludo I

[1565. Modern Ludo I](https://www.lintcode.com/problem/modern-ludo-i/?_from=ladder&&fromId=62)

#### 題目

There is a one-dimensional board with a starting point on the far left side of the board and an end point on the far right side of the board. There are several positions on the board that are connected to other positions, ie if A is connected to B, then when chess falls at position A, you can choose whether to move the chess from A to B. And the connection is one way, which means that the chess cannot move from B to A. Now given the length and connections of the board, and you have a six-sided dice\(1-6\), output the minimum steps to reach the end point.

Example Example1

```text
Input: length = 10 and connections = [[2, 10]]
Output: 1
Explanation: 
1->2 (dice)
2->10(for free)
```

Example2

```text
Input: length = 15 and connections = [[2, 8],[6, 9]]
Output: 2
Explanation: 
1->6 (dice)
6->9 (for free)
9->15(dice)
```

Notice

* the index starts from 1.
* length &gt; 1
* The starting point is not connected to any other location
* connections\[i\]\[0\] &lt; connections\[i\]\[1\]

#### 題目解說

這一題是說。

* 假設有一個一維的 board，長度為 length。
* 開始位置在這個 board 的最左邊 index 為 1。
* 結束位置在這個 board 的最右邊 idnex 為 length。
* 在這個 board 上有很多相連的 positions。
* 每個點的 connection 是單向的。也就是不能 a 至 b 後，b 又至 a。
* 給你一個 six sided dice。也就是說你每一步可以走 1 至 6。
* 每一次走 connection 是不用計算次數的。

然後問你最小可以到結束位置的次數。

```text
Input: length = 15 and connections = [[2, 8],[6, 9]]
Output: 2
Explanation: 
1->6 (dice)
6->9 (for free)
9->15(dice)
```

#### 解法 - 錯誤

會死在以下案例，這個案例應該先走幾部在到時 \[10,36\]，這條才是最佳解，看來還是要每一條路都走過，才能確認有沒有最佳解，可以考慮往 dp 的情境想。

```text
86
[[36,77],[5,54],[5,42],[31,37],[10,36],[15,66],[58,68]]
Your stdout
2:54
2:68
1:74
1:80
1:86

Output
5
Expected
4
```

```cpp
class Solution {
public:
    /**
     * @param length: the length of board
     * @param connections: the connections of the positions
     * @return: the minimum steps to reach the end
     */
    int modernLudo(int length, vector<vector<int>> &connections) {
        // Write your code here
        bool isEnd  = false;

        int start = 1;
        int end = length;
        int res = 0;

        while(start != end){

            int maxPath = 0;
            res++;

            for (int i=0; i < connections.size(); i++){
                int diff = connections[i][0] -  start;
                if(diff > 6 || diff < 0 ){
                    continue;
                }

                maxPath = max(maxPath, connections[i][1] - start);
            }

            // 代表沒有找到 connection 可走，所以就是直接走 6 步
            // 或是 
            if(maxPath == 0){
                start += min(6, end - start);
                cout << "1:" << start << endl;
            }else{
                start += maxPath;
                cout << "2:" << start << endl;
            }
        }
        return res;
    }
};
```

#### 解法 - DP

```cpp
class Solution {
public:
    /**
     * @param length: the length of board
     * @param connections: the connections of the positions
     * @return: the minimum steps to reach the end
     */
    int modernLudo(int length, vector<vector<int>> &connections) {
        // Write your code here

        // def: 走到第 i 個位置的最小步數
        vector<int> dp(length+1, INT_MAX);
        dp[1] = 0;

        for (int i=1; i <= length; i++){
            for (int j=1; j <= 6; j++){
                if(i > j){
                    dp[i] = min(dp[i], dp[i-j]+1);
                }
            }

            for (int k=0; k < connections.size(); k++){
                if(i == connections[k][1]){
                    dp[i] = min(dp[i], dp[connections[k][0]]);
                }

            }
        }

        return dp[length];
    }
};
```

#### 解法 - SPFA

