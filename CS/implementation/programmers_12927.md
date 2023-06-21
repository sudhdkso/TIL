## **야근 지수**


#### ***problem***
회사원 Demi는 가끔은 야근을 하는데요, 야근을 하면 야근 피로도가 쌓입니다. 야근 피로도는 야근을 시작한 시점에서 남은 일의 작업량을 제곱하여 더한 값입니다. Demi는 N시간 동안 야근 피로도를 최소화하도록 일할 겁니다.Demi가 1시간 동안 작업량 1만큼을 처리할 수 있다고 할 때, 퇴근까지 남은 N 시간과 각 일에 대한 작업량 works에 대해 야근 피로도를 최소화한 값을 리턴하는 함수 solution을 완성해주세요.

#### **제한사항**
- `works`는 길이 1 이상, 20,000 이하인 배열입니다.
- `works`의 원소는 50000 이하인 자연수입니다.
- `n`은 1,000,000 이하인 자연수입니다.


### ***Solution***
``` java
import java.util.*;
import java.util.stream.Collectors;
class Solution {
    public long solution(int n, int[] works) {
        long answer = 0;
        PriorityQueue<Integer> pq = new PriorityQueue<>((o1,o2) -> o2-o1);
        pq.addAll(Arrays.stream(works)   
                        .boxed()
                        .collect(Collectors.toList()));
        while(n-- > 0){
            int num = pq.poll();
            if(num <= 0) break;
            pq.offer(num-1);
        }
        while(!pq.isEmpty()){
            answer += (int)Math.floor(Math.pow(pq.poll(),2));
        }
        return answer;
    }
}
```
- 야근 시간이 클 수록, 야근의 피로도가 높아진다.
- 우선순위큐를 활용하여 가장 작업량이 가장 많은 것부터 1씩 줄여나가면서 야근의 피로도를 최소화 할 수 있도록 한다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/12927