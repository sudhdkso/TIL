## **입국심사**


### ***problem***
n명이 입국심사를 위해 줄을 서서 기다리고 있습니다. 각 입국심사대에 있는 심사관마다 심사하는데 걸리는 시간은 다릅니다.

처음에 모든 심사대는 비어있습니다. 한 심사대에서는 동시에 한 명만 심사를 할 수 있습니다. 가장 앞에 서 있는 사람은 비어 있는 심사대로 가서 심사를 받을 수 있습니다. 하지만 더 빨리 끝나는 심사대가 있으면 기다렸다가 그곳으로 가서 심사를 받을 수도 있습니다.

모든 사람이 심사를 받는데 걸리는 시간을 최소로 하고 싶습니다.

입국심사를 기다리는 사람 수 n, 각 심사관이 한 명을 심사하는데 걸리는 시간이 담긴 배열 times가 매개변수로 주어질 때, 모든 사람이 심사를 받는데 걸리는 시간의 최솟값을 return 하도록 solution 함수를 작성해주세요.
#### **제한사항**
- 입국심사를 기다리는 사람은 1명 이상 1,000,000,000명 이하입니다.
- 각 심사관이 한 명을 심사하는데 걸리는 시간은 1분 이상 1,000,000,000분 이하입니다.
- 심사관은 1명 이상 100,000명 이하입니다.


### ***Solution***
``` java
import java.util.Arrays;

class Solution {
    public long solution(int n, int[] times) {
        long answer = 0;
        Arrays.sort(times);
        long start = times[0], end = (long)times[times.length-1]*(long)n;
        
        while(start <= end){
            long mid = (start + end)/2l;
            long sum = 0;
            
            for(int time:times){
                sum+= mid/time;
            }
            
            if(sum >= n){
                answer = mid;
                end = mid - 1;
            }
            else {
                start = mid + 1;
            }
        }
        return answer;
    }
}
```

- 일단 입력값이 매우커서 일반적인 for문으로는 시간초과가 난다.
- times를 오름차순으로 정렬한다.
- start는 가장 적은 시간: times[0], end는 가장 최악의 시간으로 두고 계산을 시작한다.
    - 가장 최악의 시간의 경우 모두가 가장 오래 걸리는 심사대로 가는 것:  times[times.length-1] * n;
- 이분탐색을 진행한다.
    - mid를 계산하고 mid값을 기준으로 몇사람이 검사할 수 있는지 sum에 저장한다.
        - 이때 mid는 총시간이고, mid/time을 한 값을 더하는 이유는 mid시간동안 통과 가능한 사람을 확인하기 위해서이다.
    - sum >= n인 경우 mid시간 동안 n명을 검사 가능하지만, 아직 더 최솟값이 남아 있을 수 있기 때문에 계산을 계속한다.
        - answer에 mid의 최솟값을 저장한다.
    - sum < n인 경우는 주어진 mid시간 만큼 n명을 모두 검사하지 못하는 조건이다.
- 이분탐색을 모두 진행한 이후 answer이 값이 n명을 검사하는데 걸리는 최소시간이다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/43238