---
description: June30
---

# Validate IP Address

[468. Validate IP Address](https://leetcode.com/problems/validate-ip-address/)

#### 題目

Write a function to check whether an input string is a valid IPv4 address or IPv6 address or neither.

IPv4 addresses are canonically represented in dot-decimal notation, which consists of four decimal numbers, each ranging from 0 to 255, separated by dots \("."\), e.g.,172.16.254.1;

Besides, leading zeros in the IPv4 is invalid. For example, the address 172.16.254.01 is invalid.

IPv6 addresses are represented as eight groups of four hexadecimal digits, each group representing 16 bits. The groups are separated by colons \(":"\). For example, the address 2001:0db8:85a3:0000:0000:8a2e:0370:7334 is a valid one. Also, we could omit some leading zeros among four hexadecimal digits and some low-case characters in the address to upper-case ones, so 2001:db8:85a3:0:0:8A2E:0370:7334 is also a valid IPv6 address\(Omit leading zeros and using upper cases\).

However, we don't replace a consecutive group of zero value with a single empty group using two consecutive colons \(::\) to pursue simplicity. For example, 2001:0db8:85a3::8A2E:0370:7334 is an invalid IPv6 address.

Besides, extra leading zeros in the IPv6 is also invalid. For example, the address 02001:0db8:85a3:0000:0000:8a2e:0370:7334 is invalid.

Note: You may assume there is no extra space or special characters in the input string.

```text
Input: "172.16.254.1"
Output: "IPv4"
Explanation: This is a valid IPv4 address, return "IPv4".

Input: "2001:0db8:85a3:0:0:8A2E:0370:7334"
Output: "IPv6"
Explanation: This is a valid IPv6 address, return "IPv6".

Input: "256.256.256.256"
Output: "Neither"
Explanation: This is neither a IPv4 address nor a IPv6 address.
```

#### 解法

這題就是判斷 ipv4 與 ipv6。注意這題很多 edge case 要考慮，注意程式碼中每個判斷。

```cpp
class Solution {
private:
    bool isIPV4(string IP){
        if(IP.size() > 15){
            return false;
        }

        istringstream in(IP);
        string t;
        int count = 0;

        while(getline(in, t, '.')){
            // 判斷是否 <= 3 個字
            // 1..1.1.1 如果沒有 t.empty() 下面會出錯
            if(count > 4 || t.empty()){
                return false;
            }

            if(t.size() > 3){
                return false;
            }
            // 判斷是否為 01 or 001
            if(t.size() > 1 && t[0] == '0'){
                return false;
            }
            // 判斷是否都為數字
            for (int i=0; i < t.size(); i++){
                if(!isdigit(t[i])){
                    return false;
                }
            }

            int temp = std::stoi(t);
            if(temp < 0 || temp > 255){
                return false;
            }
            count++;
        }
        // 注意 1.1.1.1. 這種案例
        return (count == 4 && IP.back() != '.');
    };
    bool isIPV6(string IP){
        // 判斷是否在 0~9 && a~f && A~F 之間 
        istringstream in(IP);
        string t;
        int count = 0;
        while(getline(in, t, ':')){
            // 判斷是否 <= 8 段
            // 判斷每段是否為 <= 4 
            if(count > 8 || t.size() > 4 || t.empty()){
                return false;
            }
            for (int i=0; i < t.size(); i++){
                if(!(t[i] >= 'a' && t[i] <= 'f') && 
                   !(t[i] >= 'A' && t[i] <= 'F') &&
                   !(t[i] >= '0' && t[i] <= '9')
                  ){
                    return false;
                }
            }
            count++;
        }

        return (count == 8 && IP.back() != ':');
    };
public:
    string validIPAddress(string IP) {
        bool is = isIPV4(IP);
        if(isIPV4(IP)){
            return "IPv4";
        }
        if(isIPV6(IP)){
            return "IPv6";
        }

        return "Neither";
    }
};
```

