443. String Compression
Given an array of characters, compress it in-place.

The length after compression must always be smaller than or equal to the original array.

Every element of the array should be a character (not int) of length 1.

After you are done modifying the input array in-place, return the new length of the array.

 
Follow up:
Could you solve it using only O(1) extra space?

```java
// good question to practice while
class Solution {
    public int compress(char[] chars){
        int resIdx = 0, index = 0;
        while (index < chars.length){            
            int cnt = 0;
            char current = chars[index];
            while (index < chars.length && chars[index] == current){
                index++;
                cnt++;
            }
            chars[resIdx++] = current;
            if (cnt != 1){
                for (char c : Integer.toString(cnt).toCharArray()){
                    chars[resIdx++] = c;
                }
            }
            
        }
        return resIdx;
    }
}
