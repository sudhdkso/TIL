## **단속카메라**


#### ***problem***
고속도로를 이동하는 모든 차량이 고속도로를 이용하면서 단속용 카메라를 한 번은 만나도록 카메라를 설치하려고 합니다.

고속도로를 이동하는 차량의 경로 routes가 매개변수로 주어질 때, 모든 차량이 한 번은 단속용 카메라를 만나도록 하려면 최소 몇 대의 카메라를 설치해야 하는지를 return 하도록 solution 함수를 완성하세요.

#### **제한사항**
- 차량의 대수는 1대 이상 10,000대 이하입니다.
- routes에는 차량의 이동 경로가 포함되어 있으며 routes[i][0]에는 i번째 차량이 고속도로에 진입한 지점, routes[i][1]에는 i번째 차량이 고속도로에서 나간 지점이 적혀 있습니다.
- 차량의 진입/진출 지점에 카메라가 설치되어 있어도 카메라를 만난것으로 간주합니다.
- 차량의 진입 지점, 진출 지점은 -30,000 이상 30,000 이하입니다.



### ***Solution***
``` java
import java.util.*;

class Solution {
    public int solution(int[][] routes) {
        int answer = 0;
        int length = routes.length;
        Arrays.sort(routes, (o1,o2) -> o1[1]-o2[1]);
        int max = Integer.MIN_VALUE;
        for(int i=0;i<length;i++){
            if(max < routes[i][0]){
                answer++;
                max = routes[i][1];
            }
        }
        return answer;
    }
}
```
- route 배열을 진출 시기에 대해서 오름차순으로 정렬한다.
- 문제에서 진입/진출 시기에 대해서 route[i-1][1]이 route[i][0] 보다 작으면 단속카메라 설치가 필요하다.
    - 이때 max에는 route[i][1]을 저장한다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/42884