Problem:
Given a string text, you want to use the characters of text to form as many instances of the word "balloon" as possible.
You can use each character in text at most once. Return the maximum number of instances that can be formed.

Example 1:
Input: text = "nlaebolko"
Output: 1

Example 2:
Input: text = "loonbalxballpoon"
Output: 2

Example 3:
Input: text = "leetcode"
Output: 0
 

Constraints:
1 <= text.length <= 104
text consists of lower case English letters only.

Solution:

```
class Solution {
    public int maxNumberOfBalloons(String text) {
        int[] freq = new int[26];
        for(char c : text.toCharArray()) {
            freq[c-'a']++;
        }

        int[] bFreq = new int[26];
        String b = "balloon";
        for(char c : b.toCharArray()) {
            bFreq[c-'a']++;
        }

        int count = 0;
        while(contains(freq, bFreq)) {
            count++;
        }

        return count;
    }

    private boolean contains(int[] freq, int[] bfreq) {
        for(int i=0; i<bfreq.length; i++) {
            int count = bfreq[i];
            int counta = freq[i];
            if(counta < count) {
                return false;
            } else {
                freq[i] -= count;
            }
        }
        return true;
    }
}
```

TimeComplexity: O(n)
SpaceComplexity: O(1)