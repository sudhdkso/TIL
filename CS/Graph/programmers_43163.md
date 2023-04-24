## **단어 변환**


### ***problem***
두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.
```
1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.
```
예를 들어 begin이 "hit", target가 "cog", words가 ["hot","dot","dog","lot","log","cog"]라면 "hit" -> "hot" -> "dot" -> "dog" -> "cog"와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

#### **제한사항**
- 각 단어는 알파벳 소문자로만 이루어져 있습니다.
- 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
- words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
- begin과 target은 같지 않습니다.
- 변환할 수 없는 경우에는 0를 return 합니다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    public static boolean[] visited;
    public static int min = Integer.MAX_VALUE;
    public int solution(String begin, String target, String[] words) {
        int answer = 0;
        visited = new boolean[words.length];
        min = Integer.MAX_VALUE;
        dfs(begin, target, words, 0);
        if(min == Integer.MAX_VALUE){
            return 0;
        }
        return min;
    }

    public static void dfs(String begin, String target, String[] words, int count){
        int length = words.length;
        if(begin.equals(target)){
            min = Math.min(min, count);
            return;
        }

        for(int i=0;i<length;i++){
            if(!visited[i] && check(begin, words[i])){
                visited[i] = true;
                dfs(words[i], target, words, ++count);
                count--;
                visited[i] = false;
            }
        }
        return;
    }

    public static boolean check(String word1, String word2){
        int length = word1.length();
        int count = 0;
        for(int i=0;i<length;i++){
            if(word1.charAt(i) != word2.charAt(i)){
                count++;
            }
        }
        if(count == 1){
            return true;
        }
        else {
            return false;
        }
    }
}
```
- dfs를 활용하여 변환가능한 모든 경우의 수를 확인한다.
- 변환 가능하면 min을 변경해준다.
- visited는 word의 i번째의 방문 여부를 저장하는 boolean타입의 일차원 배열이다.
- check()는 word1과 word2가 알파벳 한개의 차이만 나는지 확인하는 함수이다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/43163