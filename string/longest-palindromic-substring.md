---
description: Amazon、Microsoft
---

# Longest Palindromic Substring



#### 解法 - DP  O\(N^2\), O\(N^2\)

```cpp
class Solution {
public:
    /**
     * @param s: input string
     * @return: the longest palindromic substring
     */
    string longestPalindrome(string &s) {
        // write your code here
        // def: dp[l][r] => s[l..r] 是否為 palindromic
        string res = {};
        if(s.size() == 0){
            return res;
        }

        int n = s.size();
        vector<vector<bool>> dp(n, vector<bool>(n, false));
        int max_num = INT_MIN;
        int max_l = 0;
        int max_r = 0;

        for (int l=n-1; l >= 0; l--){
            for (int r=l; r < n; r++){
                int len = r - l;
                if(s[l] == s[r] && (r - l <= 2 || dp[l+1][r-1])){
                    dp[l][r] = true;
                    if(len > max_num){
                        max_l = l;
                        max_r = r;
                        max_num = len;
                    }
                }
            }
        }


        for (int i=max_l ; i<= max_r; i++){
            res += s[i];
        }

        return res;
    }
};
```

#### 解法 - 雙指針左右展  O\(N^2\), O\(1\)

> 每一個位置都從左右兩邊展開，來判斷是否為回文。但注意要分單與雙處理。

```cpp
class Solution {
private:
    bool valid(int start, int end, int n){
        return start >= 0 && end < n;
    }
public:
    /**
     * @param s: input string
     * @return: the longest palindromic substring
     */
    string longestPalindrome(string &s) {
        // write your code here
        int n = s.size();
        int max_num = INT_MIN;
        int max_l = 0;
        int max_r = 0;

        for (int center=0; center < n; center++){
            // 單數組的求法
            for (int start=center, end = center; valid(start, end, n); start--,end++){
                if(s[start] != s[end]){
                    break;
                }

                int new_len = end - start;
                if(new_len > max_num){
                    max_l = start;
                    max_r = end;
                    max_num = new_len;
                }
            }

            // 雙數組的求法
            for (int start=center, end = center+1; valid(start,end, n); start--, end++){
                if(s[start] != s[end]){
                    break;
                }

                int new_len = end - start;
                if(new_len > max_num){
                    max_l = start;
                    max_r = end;
                    max_num = new_len;
                }
            }
        }

        string res = {};
        for (int k = max_l; k <= max_r; k++){
            res += s[k];
        }
        return res;
    }
};
```

#### 解法 - Manancher's Algorithm \( O\(N\) \)

> 這考出來應該就是不想要徵人…

[看不懂，就先這樣](https://medium.com/hoskiss-stand/manacher-299cf75db97e)

