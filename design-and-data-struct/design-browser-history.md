---
description: Contest 192
---

# Design Browser History

[1472. Design Browser History](https://leetcode.com/problems/design-browser-history/)

#### 題目

You have a browser of one tab where you start on the homepage and you can visit another url, get back in the history number of steps or move forward in the history number of steps.

Implement the BrowserHistory class:

* BrowserHistory\(string homepage\) Initializes the object with the homepage of the browser.
* void visit\(string url\) visits url from the current page. It clears up all the forward history.
* string back\(int steps\) Move steps back in history. If you can only return x steps in the history and steps &gt; x, you will return only x steps. Return the current url after moving back in history at most steps.
* string forward\(int steps\) Move steps forward in history. If you can only forward x steps in the history and steps &gt; x, you will forward only x steps. Return the current url after forwarding in history at most steps.

```text
Input:
["BrowserHistory","visit","visit","visit","back","back","forward","visit","forward","back","back"]
[["leetcode.com"],["google.com"],["facebook.com"],["youtube.com"],[1],[1],[1],["linkedin.com"],[2],[2],[7]]
Output:
[null,null,null,null,"facebook.com","google.com","facebook.com",null,"linkedin.com","google.com","leetcode.com"]

Explanation:
BrowserHistory browserHistory = new BrowserHistory("leetcode.com");
browserHistory.visit("google.com");       // You are in "leetcode.com". Visit "google.com"
browserHistory.visit("facebook.com");     // You are in "google.com". Visit "facebook.com"
browserHistory.visit("youtube.com");      // You are in "facebook.com". Visit "youtube.com"
browserHistory.back(1);                   // You are in "youtube.com", move back to "facebook.com" return "facebook.com"
browserHistory.back(1);                   // You are in "facebook.com", move back to "google.com" return "google.com"
browserHistory.forward(1);                // You are in "google.com", move forward to "facebook.com" return "facebook.com"
browserHistory.visit("linkedin.com");     // You are in "facebook.com". Visit "linkedin.com"
browserHistory.forward(2);                // You are in "linkedin.com", you cannot move forward any steps.
browserHistory.back(2);                   // You are in "linkedin.com", move back two steps to "facebook.com" then to "google.com". return "google.com"
browserHistory.back(7);                   // You are in "google.com", you can move back only one step to "leetcode.com". return "leetcode.com"
```

#### 解法 - Vector

這題一開始的想是有一個 vector 用來儲放到過的網頁，然後有一個 index 會根據 back 與 forward 來進行移動。

但這個解法在以下的案例會錯。

```text
[
"BrowserHistory", (leetcode.com)
"visit", (google.com)
"visit", (facebook.com)
"visit", (youtube.com)
"back", (1)
"back", (1)
"forward", (1)
"visit", (linkedin.com) <--- 根源在這個的新增
"forward", (2)
"back", (2)  <--- 問題這個錯，預期是 google 但是結果是 facebook。
"back" (7)
]
```

```cpp
class BrowserHistory {
private:
    vector<string> historys;     
    int cur_index;
public:
    BrowserHistory(string homepage) {
        historys.push_back(homepage);
        cur_index = 0;
    }

    void visit(string url) {
        historys.push_back(url);
        cur_index++;
    }

    string back(int steps) {
        if(cur_index < steps) {
            cur_index = 0;
            return historys[cur_index];
        };

        cur_index -= steps;
        return historys[cur_index];
    }

    string forward(int steps) {
        if(cur_index + steps >= historys.size()){
          cur_index = historys.size()-1;  
          return historys[cur_index];  
        } 
        cur_index += steps;
        return historys[cur_index];
    }
};

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory* obj = new BrowserHistory(homepage);
 * obj->visit(url);
 * string param_2 = obj->back(steps);
 * string param_3 = obj->forward(steps);
 */
```

**正解**

這裡記得要看清楚題目，它有說 visit 時會移除 forward 的記錄。

> void visit\(string url\) visits url from the current page. It clears up all the forward history.

```cpp
class BrowserHistory {
private:
    vector<string> historys;     
    int cur_index;
public:
    BrowserHistory(string homepage) {
        historys.push_back(homepage);
        cur_index = 0;
    }

    void visit(string url) {
        /* 注意這裡要移除 cur_index 後面的記錄，這樣才能解上述錯誤版的問題。
         因為題目有說
         */
        while (cur_index < historys.size() - 1) {
            historys.pop_back();
        }
        historys.push_back(url);
        cur_index++;
    }

    string back(int steps) {
        if(cur_index < steps) {
            cur_index = 0;
            return historys[cur_index];
        };

        cur_index -= steps;
        return historys[cur_index];
    }

    string forward(int steps) {
        if(cur_index + steps >= historys.size()){
          cur_index = historys.size()-1;  
          return historys[cur_index];  
        } 
        cur_index += steps;
        return historys[cur_index];
    }
};

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory* obj = new BrowserHistory(homepage);
 * obj->visit(url);
 * string param_2 = obj->back(steps);
 * string param_3 = obj->forward(steps);
 */
```

