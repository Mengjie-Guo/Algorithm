# Dynamic Programming with memory to replace Backtrack

Here I propose a new solution for [leetcode 140. Word Break II](https://leetcode.com/problems/word-break-ii/description/), reduce the time comlecity to O(n^2), while its offical and all the other solutions' is O(n*2^n).

Chinese Solution: [C++击败100%，无需回溯，在dp过程中记录字符串](https://leetcode.cn/problems/word-break-ii/solutions/2803097/cji-bai-100wu-xu-hui-su-zai-dpguo-cheng-150g9/)

## Problem
>from leetcode https://leetcode.com/problems/word-break-ii/description/
>
>Given a string s and a dictionary of strings wordDict, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in any order.
>Note that the same word in the dictionary may be reused multiple times in the segmentation.
>
>Example 1:
Input: s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
>Output: ["cats and dog","cat sand dog"]
>
>Example 2:
>Input: s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]
>Output: ["pine apple pen apple","pineapple pen apple","pine applepen apple"]
>Explanation: Note that you are allowed to reuse a dictionary word.
>
>Example 3:
>Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
>Output: []

## Solution Description 
Use dynamic programing table `dp[i]` to determine whether it can be split, the string composed at this time is recorded into the corresponding string array `record[i]`, which means all the strings composed from the beginning to i.

## Code
```
class Solution {
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string>wordSet;
        for(string w: wordDict){
            wordSet.insert(w);
        }
        int n=s.size();
        vector<bool>dp(n+1, false);
        dp[0]=true;
        vector<vector<string>>record(n+1);
        record[0]={""};
        for(int i=1;i<=n;i++){
            for(int j=0;j<i;j++){
                string after=s.substr(j,i-j);
                if(dp[j] && wordSet.count(after)){
                    dp[i]=true;
                    for(string x:record[j]){
                        string news=(x.size()>0?(x+" "):"")+after;
                        record[i].push_back(news);
                    }
                }
            }
        }
        return record[n];
    }
};
```

