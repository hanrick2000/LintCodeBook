##Word Break II

  Given a string s and a dictionary of words dict, 
  add spaces in s to construct a sentence where each word is a valid dictionary word.

  Return all such possible sentences.

##Example
  Gieve s = lintcode,
  dict = ["de", "ding", "co", "code", "lint"].

  A solution is ["lint code", "lint co de"].
  
  
##思路
- DFS，但是跑到96% 超内存了
- 应该使用记忆化搜索减小内存使用量

###超内存
```java
public class Solution {
    /*
     * @param s: A string
     * @param wordDict: A set of words.
     * @return: All possible sentences.
     */
    public List<String> wordBreak(String s, Set<String> wordDict) {
        // write your code here
        List<String> results = new ArrayList<String>();
        
        if (s == null || s.length() == 0 || wordDict.isEmpty()) {
            return results;
        }
        
        List<String> words = new ArrayList<String>(wordDict);
        Collections.sort(words);
        dfs(results, words, new StringBuilder(""), s, 0);
        
        return results;
        
    }
    
    public void dfs(List<String> results, List<String> words, StringBuilder sb, String s, int startIndex) {
        
        if (startIndex > s.length()) {
            return;
        }
        
        if (startIndex == s.length()) {
            StringBuilder sbResult = new StringBuilder(sb);
            sbResult.setLength(sbResult.length() - 1);
            results.add(sbResult.toString());
            return;
        }
            
        for (String word : words) {
            if (word.equals("")) {
                return;
            }
            int endIndex = startIndex + word.length();
            if (isMatch(s, startIndex, endIndex, word)) {
                sb.append(word);
                sb.append(" ");
                dfs(results, words, sb, s, endIndex);
                sb.setLength(sb.length() - word.length() - 1);
            }
        }
        
    }
    
    public boolean isMatch(String s, int index1, int index2, String match) {
        if (index2 > s.length()) {
            return false;
        }
        return s.substring(index1, index2).equals(match);
    }
}
```

###记忆化搜索
- 拆分问题，每个大string都由若干个小string组成，所以如果能构成每个小string，那么合在一起就能构成大string
- 这样就可以开始记忆化记录
- 记录每一个子string，需要构成的来自于dict的单词
- 所以此时for循环，不再是循环每个单词，而是循环每个index，看0-index 和index+1 - end的这两个子string是如何构成的

```java
public class Solution {
    public List<String> wordBreak(String s, Set<String> dict) {
        Map<String, List<String>> done = new HashMap<>();
        done.put("", new ArrayList<>());
        done.get("").add("");

        return dfs(s, dict, done);
    }

    List<String> dfs(String s, Set<String> dict, Map<String, List<String>> done) {
        if (done.containsKey(s)) {
            return done.get(s);
        }
        List<String> ans = new ArrayList<>();

        for (int len = 1; len <= s.length(); len++) {  //将s 分割成s1 s2  其中s1长度为len
            String s1 = s.substring(0, len);
            String s2 = s.substring(len);

            if (dict.contains(s1)) {
                List<String> s2_res = dfs(s2, dict, done);
                for (String item : s2_res) {
                    if (item == "") {
                        ans.add(s1);
                    } else {
                        ans.add(s1 + " " + item);
                    }
                }
            }
        }
        done.put(s, ans);
        return ans;
    }
}
```
