---
description: 注意、Amazon
---

# Most Common Word

[1369. Most Common Word](https://www.lintcode.com/problem/most-common-word/?_from=ladder&&fromId=126)

#### 題目

Given a paragraph and a list of banned words, return the most frequent word that is not in the list of banned words. It is guaranteed there is at least one word that isn't banned, and that the answer is unique.

Words in the list of banned words are given in lowercase, and free of punctuation. Words in the paragraph are not case sensitive. The answer is in lowercase.

Example Example1

```text
Input:  paragraph = "Bob hit a ball, the hit BALL flew far after it was hit." and banned = ["hit"]
Output: "ball"
Explanation:
"hit" occurs 3 times, but it is a banned word.
"ball" occurs twice (and no other word does), so it is the most frequent non-banned word in the paragraph. 
Note that words in the paragraph are not case sensitive,
that punctuation is ignored (even if adjacent to words, such as "ball,"), 
and that "hit" isn't the answer even though it occurs more because it is banned.
```

Example2

```text
Input:  paragraph = "a a a b b c c d" and banned = ["a","b"]
Output: "c"
Explanation:
"a" and "b" are banned words
"c" occurs 2 times and "d" occurs only once
So output "c"
```

Notice

* 1 &lt;= paragraph.length &lt;= 1000.
* 1 &lt;= banned.length &lt;= 100.
* 1 &lt;= banned\[i\].length &lt;= 10.
* The answer is unique, and written in lowercase \(even if its occurrences in paragraph may have uppercase symbols, and even if it is a proper noun.\)
* paragraph only consists of letters, spaces, or the punctuation symbols !?',;.
* Different words in paragraph are always separated by a space.
* There are no hyphens or hyphenated words.
* Words only consist of letters, never apostrophes or other punctuation symbols.

#### 解法 - Map

```cpp
class Solution {
private:
    bool isNotBannedWord(string s, vector<string>& banned){
        if(banned.size() == 0){
            return true;
        }

        for (int i=0; i < banned.size(); i++){
            if(s == banned[i]){
                return false;
            }
        }
        return true;
    }
public:
    /**
     * @param paragraph: 
     * @param banned: 
     * @return: nothing
     */
    string mostCommonWord(string &paragraph, vector<string> &banned) {
        string res;
        // step 1. Clear the not alpha word like , or . 
        for (auto &c : paragraph){
            c = isalpha(c) ? tolower(c) : ' ';
        }

        unordered_map<string, int> map;
        istringstream in(paragraph);
        string temp;
        pair<string, int> frequentString = {"", 0};
        while (getline(in, temp, ' ')) {
            if(temp.find_first_not_of(' ') != std::string::npos)
            {
                if(isNotBannedWord(temp, banned)){
                    map[temp] += 1;
                }
            }

            if(frequentString.second < map[temp]){
                frequentString = {temp, map[temp]};
            }
        }

        return frequentString.first;
    }
};
```

