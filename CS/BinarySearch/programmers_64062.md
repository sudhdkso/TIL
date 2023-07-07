## **징검다리 건너기**


### ***problem***
카카오 초등학교의 "니니즈 친구들"이 "라이언" 선생님과 함께 가을 소풍을 가는 중에 **징검다리**가 있는 개울을 만나서 건너편으로 건너려고 합니다. "라이언" 선생님은 "니니즈 친구들"이 무사히 징검다리를 건널 수 있도록 다음과 같이 규칙을 만들었습니다.

- 징검다리는 일렬로 놓여 있고 각 징검다리의 디딤돌에는 모두 숫자가 적혀 있으며 디딤돌의 숫자는 한 번 밟을 때마다 1씩 줄어듭니다.
- 디딤돌의 숫자가 0이 되면 더 이상 밟을 수 없으며 이때는 그 다음 디딤돌로 한번에 여러 칸을 건너 뛸 수 있습니다.
- 단, 다음으로 밟을 수 있는 디딤돌이 여러 개인 경우 무조건 가장 가까운 디딤돌로만 건너뛸 수 있습니다.

"니니즈 친구들"은 개울의 왼쪽에 있으며, 개울의 오른쪽 건너편에 도착해야 징검다리를 건넌 것으로 인정합니다.
"니니즈 친구들"은 한 번에 한 명씩 징검다리를 건너야 하며, 한 친구가 징검다리를 모두 건넌 후에 그 다음 친구가 건너기 시작합니다.

디딤돌에 적힌 숫자가 순서대로 담긴 배열 stones와 한 번에 건너뛸 수 있는 디딤돌의 최대 칸수 k가 매개변수로 주어질 때, 최대 몇 명까지 징검다리를 건널 수 있는지 return 하도록 solution 함수를 완성해주세요.

### **[제한사항]**
- 징검다리를 건너야 하는 니니즈 친구들의 수는 무제한 이라고 간주합니다.
- stones 배열의 크기는 1 이상 200,000 이하입니다.
- stones 배열 각 원소들의 값은 1 이상 200,000,000 이하인 자연수입니다.
- k는 1 이상 stones의 길이 이하인 자연수입니다.

### ***Solution***
``` java
class Solution {
    public int solution(int[] stones, int k) {
        int answer = 0;
        int low = 1, high =  200_000_000;
        int mid = 0;
        while(low <= high){
            mid = (low+high)/2;
            if(isCross(stones,k,mid)){
                answer = Math.max(answer, mid);
                low = mid+1;
            }
            else{
                high = mid-1;
            }
        }
        return answer;
    }
    
    private static boolean isCross(int[] stones, int k, int mid){
        int count = 0;
        for(int stone : stones){
            if(mid > stone){
                count++;
            }
            else{
                count = 0;
            }
            if(count == k){
                return false;
            }
        }
        return true;
    }
}
```

- 이분 탐색을 활용하는 문제이다.
- isCross()는 매개변수로 받은 mid명의 인원이 징검다리를 건널 수 있는지 boolean타입으로 return해주는 함수이다.   
    - isCross함수에서 연속적으로 k번 mid명이 징검다리를 건널 수 없는 돌이 나오는 경우는 징검다리를 건널 수 없다.
- 이분탐색 과정
1. isCross()함수를 통해 mid명이 징검다리를 건널 수 있다면 `low = mid+1`로 하고 answer는 mid가 그전 answer보다 크다면 갱신해준다.
2. mid명이 징검다리를 건널 수 없다면 `high = mid-1`로 한다.
3. 그리고 위 과정을 low가 high보다 작거나 같을 때까지 계속 반복하며 가장 큰 mid값을 찾아나간다.

- 이분탐색을 통해 얻은 answer를 return한다.
### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/64062