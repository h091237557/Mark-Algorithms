# First Unique Character in a String

[209. First Unique Character in a String](https://www.lintcode.com/problem/first-unique-character-in-a-string/?_from=ladder&&fromId=59)

#### 題目

Find the first unique character in a given string. You can assume that there is at least one unique character in the string.

Example

```text
Example 1:
    Input: "abaccdeff"
    Output:  'b'

    Explanation:
    There is only one 'b' and it is the first one.


Example 2:
    Input: "aabccd"
    Output:  'b'

    Explanation:
    'b' is the first one.
```

#### 解法 - Map

```cpp
class Solution {
public:
    /**
     * @param str: str: the given string
     * @return: char: the first unique character in a given string
     */
    char firstUniqChar(string &str) {

        if(str.size() == 0){
            return '0';
        }

        unordered_map<char, pair<bool, int>> map;

        for (int i=0; i < str.size(); i++){
            // defaut set true;
            if(!map.count(str[i])){
                map[str[i]] = make_pair(true, i);
            }else{
                map[str[i]] = make_pair(false,-1);
            }
        }

        char res;
        int index = INT_MAX;
        for (auto it: map){
            char s = it.first;
            pair<bool,int> element = it.second;
            if(element.first == true){
                if(element.second <= index){
                    res = s;
                    index = element.second;
                }
            }
        }
        return res;
    }
};
```

#### 解法 - 字母

```cpp
char firstUniqChar(string &str) {
    // Write your code here
    int vis[256] = {0};
    for(int i = 0; i < str.size(); i++) {
        vis[str[i]]++;
    }

    for(int i = 0; i < str.size(); i++) {
        if(vis[str[i]] == 1){
            return str[i];
        }
    }

}
```

