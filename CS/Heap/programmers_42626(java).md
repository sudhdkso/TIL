## **더 맵게**


### ***problem***
매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.
``` markdown
섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
```
Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

#### **제한사항**
- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

### ***Solution***
``` java
import java.util.*;
import java.util.stream.Collectors;
class Solution {
    public int solution(int[] scoville, int K) {
        int answer = 0;
        PriorityQueue<Integer> pq = new PriorityQueue<>(Arrays.stream(scoville).boxed().collect(Collectors.toList()));

        while(pq.size() > 1 && pq.peek() < K){
            int first = pq.poll();
            int second = pq.poll();
            pq.offer(first+ second*2);
            answer++;
            
            
        }
        
        if(pq.peek() < K){
            answer = -1;
        }
        return answer;
    }
}
```
### **문제 풀이** 
- 우선순위 큐를 활용하여 가장 작은 것과 두번째 작은 스코빌 지수를 통해서 새로운 스코빌 지수를 만든다.
- 큐의 가장 앞이 K보다 커지거나 큐에 하나의 값이 남을때 까지 위의 행동을 반복한다.
    - 이때 한번 행동할 때 마다 answer를 1증가시킨다.
- 큐의 가장 앞에 있는 값이 K보다 작으면 스코빌 지수가 모두 K이상이 될 수 없으므로 -1을 return한다.
- 그게 아니면 answer를 return한다.
    
### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42626