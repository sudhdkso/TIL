## **기능개발**


### ***problem***
트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.

예를 들어, 트럭 2대가 올라갈 수 있고 무게를 10kg까지 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.
<table class="table">
        <thead><tr>
<th>경과 시간</th>
<th>다리를 지난 트럭</th>
<th>다리를 건너는 트럭</th>
<th>대기 트럭</th>
</tr>
</thead>
        <tbody><tr>
<td>0</td>
<td>[]</td>
<td>[]</td>
<td>[7,4,5,6]</td>
</tr>
<tr>
<td>1~2</td>
<td>[]</td>
<td>[7]</td>
<td>[4,5,6]</td>
</tr>
<tr>
<td>3</td>
<td>[7]</td>
<td>[4]</td>
<td>[5,6]</td>
</tr>
<tr>
<td>4</td>
<td>[7]</td>
<td>[4,5]</td>
<td>[6]</td>
</tr>
<tr>
<td>5</td>
<td>[7,4]</td>
<td>[5]</td>
<td>[6]</td>
</tr>
<tr>
<td>6~7</td>
<td>[7,4,5]</td>
<td>[6]</td>
<td>[]</td>
</tr>
<tr>
<td>8</td>
<td>[7,4,5,6]</td>
<td>[]</td>
<td>[]</td>
</tr>
</tbody>
</table>

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.


### **제한사항**
- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

### ***Solution***
``` java
import java.util.*;
import java.util.stream.Collectors;
class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        int answer = 0;
        int sum = 0;
        Queue<Integer> bridge = new LinkedList<>();
        Queue<Integer> truck = new LinkedList<>(Arrays.stream(truck_weights).boxed().collect(Collectors.toList()));
        

        while(!truck.isEmpty()){
            if(bridge.size() < bridge_length){
                answer++;
                if(sum + truck.peek() <= weight){    
                    sum += truck.peek();
                    bridge.offer(truck.poll());
                }
                else{
                    bridge.offer(0);
                }
            }
            else{
                sum -= bridge.poll();
            }
        }

        answer += bridge_length;

        
        return answer;
    }
}
```
### **문제 풀이** 
- truck과 bridge를 큐로 표현한다.
- 대기하고 있는 truck 큐의 맨앞에 truck이 bridge에 올라 갈 수 있는지 확인한다.
    - bridge의 size가 bridge의 length보다 작아야한다.
- 올라갈 수 있으면 bridge에 있는 무게와 이제 올라가 트럭(trucks.peek())의 합이 weight보다 작거나 같은지 확인한다.
    - 트럭이 다리위에 올라갈 수 있으면 sum에 해당 무게를 더하고, bridge에 트럭의 무게를 추가한다.
    - 트럭이 다리위에 올라갈 수 없는 무게이면, 남은 공간에다가 임의의 0을 넣는다.
- bridge의 size가 bridge_length랑 같으면 이제 더이상 트럭이 올라갈 수 없고, 다리에서 트럭이 빠져나가야 하므로, sum에 bridge의 맨앞값을 poll()을 통해 빼준다.
- 모든 트럭이 다 다리를 지나갔거나 다리위로 올라갔다면, 위의 반복을 종료한다.
- 가장 마지막에 올라간 트럭은 bridge의 맨 끝에 위치하므로, 가장 마지막에 올라간 트럭이 지나가는데 걸리는 시간은 bridge_length만큼이기에 answer에 bridge_length를 더한다.
### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42583