## **[카카오 인턴] 보석 쇼핑**


#### ***problem***
[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]

개발자 출신으로 세계 최고의 갑부가 된 `어피치`는 스트레스를 받을 때면 이를 풀기 위해 오프라인 매장에 쇼핑을 하러 가곤 합니다.

어피치는 쇼핑을 할 때면 매장 진열대의 특정 범위의 물건들을 모두 싹쓸이 구매하는 습관이 있습니다.

어느 날 스트레스를 풀기 위해 보석 매장에 쇼핑을 하러 간 어피치는 이전처럼 진열대의 특정 범위의 보석을 모두 구매하되 특별히 아래 목적을 달성하고 싶었습니다.

`진열된 모든 종류의 보석을 적어도 1개 이상 포함하는 가장 짧은 구간을 찾아서 구매`

예를 들어 아래 진열대는 4종류의 보석(RUBY, DIA, EMERALD, SAPPHIRE) 8개가 진열된 예시입니다.

|진열대 번호	|1	|2	|3	|4	|5	|6	|7	|8 |
|---|---|---|---|---|---|---|---|---|
|보석 이름|	DIA|	RUBY|	**RUBY**|	**DIA**|	**DIA**|	**EMERALD**|	**SAPPHIRE**|	DIA|

진열대의 3번부터 7번까지 5개의 보석을 구매하면 모든 종류의 보석을 적어도 하나 이상씩 포함하게 됩니다.

진열대의 3, 4, 6, 7번의 보석만 구매하는 것은 중간에 특정 구간(5번)이 빠지게 되므로 어피치의 쇼핑 습관에 맞지 않습니다.

진열대 번호 순서대로 보석들의 이름이 저장된 배열 gems가 매개변수로 주어집니다. 이때 모든 보석을 하나 이상 포함하는 가장 짧은 구간을 찾아서 return 하도록 solution 함수를 완성해주세요.

가장 짧은 구간의 `시작 진열대 번호`와 `끝 진열대 번호`를 차례대로 배열에 담아서 return 하도록 하며, 만약 가장 짧은 구간이 여러 개라면 시작 `진열대 번호`가 가장 작은 구간을 return 합니다.

#### **[제한사항]**
- gems 배열의 크기는 1 이상 100,000 이하입니다.
    - gems 배열의 각 원소는 진열대에 나열된 보석을 나타냅니다.
    - gems 배열에는 1번 진열대부터 진열대 번호 순서대로 보석이름이 차례대로 저장되어 있습니다.
    - gems 배열의 각 원소는 길이가 1 이상 10 이하인 알파벳 대문자로만 구성된 문자열입니다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    public int[] solution(String[] gems) {
        int[] answer = new int[2];
        Set<String> set = new HashSet<>(Arrays.asList(gems));
        int k = set.size();
        int length = Integer.MAX_VALUE;
        Map<String, Integer> map = new HashMap<>();
        int left = 0;
        for(int i=0;i<gems.length;i++){
            map.put(gems[i],map.getOrDefault(gems[i],0)+1);
            
            if(map.size() >= k){
                while(map.get(gems[left]) > 1){
                    map.put(gems[left],map.get(gems[left])-1);
                    left++;
                }
                
                if(length > i-left){
                    length = i-left;
                    answer[0] = left+1;
                    answer[1] = i+1;
                }
            }
        }
        return answer;
    }
}
```
- set을 이용하여 전체 보석의 종류가 몇개인지 확인한다.
- 그리고 map에 보석을 하나씩 넣으면서, 기존에 map에 존재하는 보석이면 갯수를 증가시킨다.
- 이때 map의 size가 보석의 종류(k)와 같으면 left를 활용하여 모든 종류의 보석을 살 수 있으면서 가장 짧은 구간을 구한다.
 

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/67258