## **가장 먼 노드**


### ***problem***
조이스틱으로 알파벳 이름을 완성하세요. 맨 처음엔 A로만 이루어져 있습니다.

ex) 완성해야 하는 이름이 세 글자면 AAA, 네 글자면 AAAA

조이스틱을 각 방향으로 움직이면 아래와 같습니다.
```
▲ - 다음 알파벳
▼ - 이전 알파벳 (A에서 아래쪽으로 이동하면 Z로)
◀ - 커서를 왼쪽으로 이동 (첫 번째 위치에서 왼쪽으로 이동하면 마지막 문자에 커서)
▶ - 커서를 오른쪽으로 이동 (마지막 위치에서 오른쪽으로 이동하면 첫 번째 문자에 커서)
```

예를 들어 아래의 방법으로 "JAZ"를 만들 수 있습니다.

```
- 첫 번째 위치에서 조이스틱을 위로 9번 조작하여 J를 완성합니다.
- 조이스틱을 왼쪽으로 1번 조작하여 커서를 마지막 문자 위치로 이동시킵니다.
- 마지막 위치에서 조이스틱을 아래로 1번 조작하여 Z를 완성합니다.
```
따라서 11번 이동시켜 "JAZ"를 만들 수 있고, 이때가 최소 이동입니다.

만들고자 하는 이름 name이 매개변수로 주어질 때, 이름에 대해 조이스틱 조작 횟수의 최솟값을 return 하도록 solution 함수를 만드세요.

### **제한사항**
- name은 알파벳 대문자로만 이루어져 있습니다.
- name의 길이는 1 이상 20 이하입니다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    public int solution(String name) {
        int answer = 0;
        int len = name.length();
        int cursor = len-1;
        
        for(int i=0;i<len;i++){
            char ch = name.charAt(i);
            answer += Math.min((int)ch-(int)'A',(int)'Z'-(int)ch+1);
            
            int index = i+1;
            //A연속 구간 체크
            while(index < len && name.charAt(index) == 'A'){
                index++;
            }
            cursor = Math.min(i+(len-index)*2 , Math.min((i*2)+len-index,cursor));
            
        }
        
        
        return answer + cursor;
    }

}
```
- `cursor`는 커서를 움직이는 횟수를 의미하며, `cursor`가 될 수 있는 최댓값은 `name.length()-1`이다.
- [0,len-1]까지 i로 반복문을 돌면서 각 자리에서 해당하는 `name.charAt(i)`를 만들기 위해 버튼을 눌러야하는 횟수를 `answer`에 저장한다.
    - 버튼을 누르는 경우는 두가지 경우의 수가 존재하는데
        1. 'A'부터 `name.charAt(i)`를 만들기
        2. 'A'에서 'Z'로 갔다가 'Z'에서 `name.charAt(i)`만들기
    - 두가지 경우의 수 중 더 적게 버튼을 누르는 경우를 `answer`에 더한다.
- 그리고 i+1부터 A가 연속하여 등장하는지 확인한다.
    - A가 연속적으로 등장하는 경우는 굳이 커서가 갈 필요가 없기 때문에 
- A가 연속적으로 등장하는 곳의 마지막을 `index`로 할때 커서를 움직이는 경우는 두가지가 존재한다.
    1.  <b>`(i*2)+len-index`</b>의 경우 맨앞에서 i까지 커서를 움직였다가 다시 맨 뒤로 가기 위해서 한번더 i번 버튼을 누르고 맨뒤에서 index까지 커서를 움직이는 경우의 버튼을 누른 횟수이다.
        - 커서를 오른쪽으로 갔다가 다시 왼쪽으로 움직여 맨뒤로 이동하는 경우
    2. <b>`i+(len-index)*2`</b>의 경우 맨앞에서 맨뒤로 갔다가 index까지 이동했다가 다시 맨앞으로 가서 i까지 커서를 움직이는 경우의 버튼을 누른 횟수이다.
        - 맨뒤에서 왼쪽으로 이동하다 오른쪽으로 이동하여 맨앞에서 계속 오른쪽으로 이동하는 경우
- 두 경우의 수 중 최소로 커서를 움직이는 경우를 `cursor` 변수에 저장한다.
- 이런식으로 [0~len-1]까지 반복문을 돌면 `answer`에는 알파벳을 만들기 위해 조이스틱을 움직인 횟수, `cursor`에는 자리를 이동한 횟수가 저장되므로 정답은 `answer+cursor`를 한 값이 된다.
### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42860#