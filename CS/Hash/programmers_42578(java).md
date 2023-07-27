## **의상**


### ***problem***
코니는 매일 다른 옷을 조합하여 입는것을 좋아합니다.

예를 들어 코니가 가진 옷이 아래와 같고, 오늘 코니가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야합니다.

<table class="table">
        <thead><tr>
<th>종류</th>
<th>이름</th>
</tr>
</thead>
        <tbody><tr>
<td>얼굴</td>
<td>동그란 안경, 검정 선글라스</td>
</tr>
<tr>
<td>상의</td>
<td>파란색 티셔츠</td>
</tr>
<tr>
<td>하의</td>
<td>청바지</td>
</tr>
<tr>
<td>겉옷</td>
<td>긴 코트</td>
</tr>
</tbody>
</table>

- 코니는 각 종류별로 최대 1가지 의상만 착용할 수 있습니다. 예를 들어 위 예시의 경우 동그란 안경과 검정 선글라스를 동시에 착용할 수는 없습니다.
- 착용한 의상의 일부가 겹치더라도, 다른 의상이 겹치지 않거나, 혹은 의상을 추가로 더 착용한 경우에는 서로 다른 방법으로 옷을 착용한 것으로 계산합니다.
- 코니는 하루에 최소 한 개의 의상은 입습니다.

코니가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

#### **제한사항**
- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 코니가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    public int solution(String[][] clothes) {
        int answer = 1;
        int len = clothes.length;
        Map<String, Integer> map = new HashMap<>();
        
        for(int i=0;i<len;i++){
            String type = clothes[i][1];
            map.put(type, map.getOrDefault(type,0)+1);
        }
        
        for(String type : map.keySet()){
            answer *= map.get(type)+1;
        }
        
        answer -= 1;
        return answer;
    }
}
```
### **문제 풀이** 
- map을 활용해 의상 종류를 key, 의상의 갯수를 value로 나타낸다.
    - 의상의 이름은 착용할 의상을 결정하는데는 아무런 영향을 미치지 않고, 의상의 종류의 갯수가 중요하다.
- 입력받은 의상들을 의상 타입에 맞게 key와 value로 잘 map에 저장한다.

- 입력받은 의상의 종류로 만들 수 있는 조합의 갯수는 `각 의상의 갯수+1`한 값을 모두 곱한 값이다.
    - 각 의상에 갯수에 1을 더하는 이유는 해당 부위를 착용하지 않는 경우의 수를 포함하는 것이다.
- 그리고 answer를 return하기 전 answer에서 1을 빼준다.
    - 이는 위 처럼 `각 의상의 갯수+1`을 모두 곱한 값에는 모든 의상의 부위를 착용하지 않는 경우의 수 1개가 포함되어 있기 때문에 마지막 답을 return하기 전에 1을 빼줘야한다.
    
### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42578?language=java