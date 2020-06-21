---
description: Contest194
---

# Making File Names Unique

[1487. Making File Names Unique](https://leetcode.com/problems/making-file-names-unique/)

#### 題目

Given an array of strings names of size n. You will create n folders in your file system such that, at the ith minute, you will create a folder with the name names\[i\].

Since two files cannot have the same name, if you enter a folder name which is previously used, the system will have a suffix addition to its name in the form of \(k\), where, k is the smallest positive integer such that the obtained name remains unique.

Return an array of strings of length n where ans\[i\] is the actual name the system will assign to the ith folder when you create it.

```text
Input: names = ["pes","fifa","gta","pes(2019)"]
Output: ["pes","fifa","gta","pes(2019)"]
Explanation: Let's see how the file system creates folder names:
"pes" --> not assigned before, remains "pes"
"fifa" --> not assigned before, remains "fifa"
"gta" --> not assigned before, remains "gta"
"pes(2019)" --> not assigned before, remains "pes(2019)"
```

```text
Input: names = ["gta","gta(1)","gta","avalon"]
Output: ["gta","gta(1)","gta(2)","avalon"]
Explanation: Let's see how the file system creates folder names:
"gta" --> not assigned before, remains "gta"
"gta(1)" --> not assigned before, remains "gta(1)"
"gta" --> the name is reserved, system adds (k), since "gta(1)" is also reserved, systems put k = 2. it becomes "gta(2)"
"avalon" --> not assigned before, remains "avalon"
```

```text
Input: names = ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece"]
Output: ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece(4)"]
Explanation: When the last folder is created, the smallest positive valid k is 4, and it becomes "onepiece(4)".
```

```text
Input: names = ["wano","wano","wano","wano"]
Output: ["wano","wano(1)","wano(2)","wano(3)"]
Explanation: Just increase the value of k each time you create folder "wano".
```

```text
Input: names = ["kaido","kaido(1)","kaido","kaido(1)"]
Output: ["kaido","kaido(1)","kaido(2)","kaido(1)(1)"]
Explanation: Please note that system adds the suffix (k) to current name even it contained the same suffix before.
```

```text
Input:
["kaido","kaido(1)","kaido","kaido(1)","kaido(2)"]
Output:
["kaido","kaido(1)","kaido(2)","kaido(1)(1)","kaido(2)"]
Expected:
["kaido","kaido(1)","kaido(2)","kaido(1)(1)","kaido(2)(1)"]
```

Constraints:

* 1 &lt;= names.length &lt;= 5 \* 10^4
* 1 &lt;= names\[i\].length &lt;= 20
* names\[i\] consists of lower case English letters, digits and/or round brackets.

**題目說明**

這題是問說，給定 n 個資料夾名稱，然後當發現有重複的要將它加上括號與序列。例如下範例。

```text
Input:
["kaido","kaido(1)","kaido","kaido(1)","kaido(2)"]
```

* 第一個 kaido : 不用修改，因為前面沒重複的名稱。
* 第二個 kaido\(1\) : 不用修改，因為前面沒重複的名稱。
* 第三個 kaido : 有重複，所以將他修改成 kaido\(2\)，注意 kaido\(1\) 前面有用過了。
* 第四個 kaido\(1\) : 有重複，所以將他修改成 kaido\(1\)\(1\)
* 第五個 kaido\(2\) : 有重複，注意，在第三個修改後，就有出現 kaido\(2\)，所以他要改成 kaido\(2\)\(1\)。

#### 解法 - Map

```cpp
class Solution {
public:
    vector<string> getFolderNames(vector<string>& names) {
        unordered_map<string, int> map;

        for (int j=0; j < names.size(); j++){
            if(map.count(names[j])){
                string new_str = "";
                // 注意這裡要將新產生的字串，在去比對看看沒有沒重複。
                while(new_str.size() == 0 || map.count(new_str)){
                    int index = map[names[j]];
                    new_str = names[j] + "(" + to_string(index) + ")"; 
                    map[names[j]]++;
                }
                names[j] = new_str;
                map[new_str]++;
            }else{
                map[names[j]]++;
            }
        }
        return names;
    }
};
```

