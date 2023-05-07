## **모음사전**


### ***problem***
사전에 알파벳 모음 'A', 'E', 'I', 'O', 'U'만을 사용하여 만들 수 있는, 길이 5 이하의 모든 단어가 수록되어 있습니다. 사전에서 첫 번째 단어는 "A"이고, 그다음은 "AA"이며, 마지막 단어는 "UUUUU"입니다.

단어 하나 word가 매개변수로 주어질 때, 이 단어가 사전에서 몇 번째 단어인지 return 하도록 solution 함수를 완성해주세요.
#### **제한사항**
- word의 길이는 1 이상 5 이하입니다.
- word는 알파벳 대문자 'A', 'E', 'I', 'O', 'U'로만 이루어져 있습니다.
### ***Solution***
``` java
import java.util.*;
class Solution {
    public static ArrayList<String> wordList = new ArrayList<>();
    public int solution(String word) {
        makeWord("");
        Collections.sort(wordList);
        return wordList.indexOf(word)+1;
    }

    public static void makeWord(String s){
        String[] words = {"A","E","I","O","U"};
        int length = words.length;
        if(!s.equals("")){
            if(!wordList.contains(s)){
                wordList.add(s);
            }
        }
        if(s.length() >= 5){
            return;
        }
        for(int i=0;i<length;i++){
            makeWord(s+words[i]);
        }
    }
}
```
- makeWord()는 5개 이하의 모음으로만 이루어진 단어들의 경우의 수를 모두 만들어서 wordList에 추가하는 재귀함수이다.
- wordList에 있는 단어인지 확인하고 단어를 모두 추가한다.
- wordList를 정렬하면 문제에서 요구하는 단어의 순서대로 정렬이 된다.
- 문제에서 주어진 word의 index를 wordList에 찾은 후 +1을 하여 retrun한다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/84512