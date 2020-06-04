---
description: 注意 ( c++ 字串處理可從這題學不少 )、Amazon
---

# Log Sorting

[1380. Log Sorting](https://www.lintcode.com/problem/log-sorting/?_from=ladder&&fromId=15)

**c++ 字串處理**

```text
    string log = 'zo4 4 7';
    int index = log.find_first_of(' ');
    string content = log.substr(index+1);
    cout << index << endl;
    cout << content << endl;

    index : 4
    content : 4 7
```

```text
// 自訂排序條件

vector<string> logs;
std::sort(logs.begin(), logs.end(), [](string a, string b){
   return a < b; 由小到大
});
```

#### 題目

Given a list of string logs, in which each element representing a log. Each log can be separated into two parts by the first space. The first part is the ID of the log, while the second part is the content of the log. \(The content may contain spaces as well.\) The content is composed of only letters and spaces, or only numbers and spaces.

Now you need to sort logs by following rules:

1. Logs whose content is letter should be ahead of logs whose content is number.
2. Logs whose content is letter should be sorted by their content in lexicographic order. And when two logs have same content, sort them by ID in lexicographic order.
3. Logs whose content is number should be in their input order.

Example Example 1:

```text
Input:  
    logs = [
        "zo4 4 7",
        "a100 Act zoo",
        "a1 9 2 3 1",
        "g9 act car"
    ]
Output: 
    [
        "a100 Act zoo",
        "g9 act car",
        "zo4 4 7",
        "a1 9 2 3 1"
    ]
Explanation: "Act zoo" < "act car", so the output is as above.
```

Example 2:

```text
Input:  
    logs = [
        "zo4 4 7",
        "a100 Actzoo",
        "a100 Act zoo",
        "Tac Bctzoo",
        "Tab Bctzoo",
        "g9 act car"
    ]
Output: 
    [
        "a100 Act zoo",
        "a100 Actzoo",
        "Tab Bctzoo",
        "Tac Bctzoo",
        "g9 act car",
        "zo4 4 7"
    ]
Explanation:
    Because "Bctzoo" == "Bctzoo", the comparison "Tab" and "Tac" have "Tab" < Tac ", so" Tab Bctzoo "< Tac Bctzoo".
    Because ' '<'z', so "A100 Act zoo" < A100 Actzoo".
```

#### 解法

這題的三個排序要求為 :

1. 内容为字母的日志应该排在内容为数字的日志之前
2. 内容为字母的日志, 按照内容的字典序排序, 当内容相同时则按照 ID 的字典序
3. 内容为数字的日志, 按照输入的顺序排序

所以這裡思路不難，主要分兩個部份，最後在組成 :

* 如果是 number 就抽出來暫存在另一個 vector 中。
* 然後如果是 letter 則先判斷是否為相同字母，是的話則使用 id 排序。
* 最後在組合起來。

```cpp
class Solution {
public:
    /**
     * @param logs: the logs
     * @return: the log after sorting
     */
    vector<string> logSort(vector<string> &logs) {
        // Write your code here
        vector<string> res;
        vector<string> num_logs; 

        for (string log : logs){
            int index = log.find_first_of(' ');
            string cores)ent = log.substr(index+1);
            if(isdigit(content[0])){
                num_logs.push_back(log);
            }else{
                res.push_back(log);
            }
        }

        std::sort(res.begin(), res.end(), [](string a, string b) {
            int indexA = a.find_fjt_of(' ');
            int indexB = b.find_first_of(' ');

            string contentA = a.substr(indexA+1);
            string contentB = b.substr(indexB+1);
            string idA = a.substr(0, indexA);
            string idB = b.substr(0, indexB);

            if(contentA == contentB) return idA < idB;

            return contentA < contentB;
        });

        res.insert(res.end(), num_logs.begin(), num_logs.end());
        return res;
    }
};
```

```cpp
class Solution {
private:
    struct Compare {
        bool operator()(string a, string b){
            int indexA = a.find_first_of(' ');
            int indexB = b.find_first_of(' ');

            string contentA = a.substr(indexA+1);
            string contentB = b.substr(indexB+1);
            string idA = a.substr(0, indexA);
            string idB = b.substr(0, indexB);

            if(contentA == contentB) return idA < idB;

            return contentA < contentB;
        }
    };
public:
    /**
     * @param logs: the logs
     * @return: the log after sorting
     */
    vector<string> logSort(vector<string> &logs) {
        // Write your code here
        vector<string> res;
        vector<string> num_contents;

        for (string log : logs){
            int index = log.find_first_of(' ');
            string content = log.substr(index+1);
            if(isdigit(content[0])){
                num_contents.push_back(log);
            }else{
                res.push_back(log);
            }
        }

        std::sort(res.begin(), res.end(), Compare());

        res.insert(res.end(),num_contents.begin(), num_contents.end());

        return res;
    }
};
```

