## **섬 연결하기**


### ***problem***
n개의 섬 사이에 다리를 건설하는 비용(costs)이 주어질 때, 최소의 비용으로 모든 섬이 서로 통행 가능하도록 만들 때 필요한 최소 비용을 return 하도록 solution을 완성하세요.

다리를 여러 번 건너더라도, 도달할 수만 있으면 통행 가능하다고 봅니다. 예를 들어 A 섬과 B 섬 사이에 다리가 있고, B 섬과 C 섬 사이에 다리가 있으면 A 섬과 C 섬은 서로 통행 가능합니다.


### **제한사항**
- 섬의 개수 n은 1 이상 100 이하입니다.
- costs의 길이는 ((n-1) * n) / 2이하입니다.
- 임의의 i에 대해, costs[i][0] 와 costs[i] [1]에는 다리가 연결되는 두 섬의 번호가 들어있고, costs[i] [2]에는 이 두 섬을 연결하는 다리를 건설할 때 드는 비용입니다.
- 같은 연결은 두 번 주어지지 않습니다. 또한 순서가 바뀌더라도 같은 연결로 봅니다. 즉 0과 1 사이를 연결하는 비용이 주어졌을 때, 1과 0의 비용이 주어지지 않습니다.
- 모든 섬 사이의 다리 건설 비용이 주어지지 않습니다. 이 경우, 두 섬 사이의 건설이 불가능한 것으로 봅니다.
- 연결할 수 없는 섬은 주어지지 않습니다.

### ***Solution***
``` java
import java.util.Arrays;
class Solution {
    static int[][] list;
    public int solution(int n, int[][] costs) {
        int answer = 0;
        int len = costs.length;
        boolean[] visited = new boolean[n];
        int distance[] = new int[n];
        
        list = new int[n][n];
        
        for(int i=0;i<len;i++){
            list[costs[i][0]][costs[i][1]] = costs[i][2];
            list[costs[i][1]][costs[i][0]] = costs[i][2];
        }
        
		Arrays.fill(distance, Integer.MAX_VALUE);
        
        distance[0] = 0;
        int count = 0;
        
        while(true){
            int min = Integer.MAX_VALUE;
            int index = 0;
            for(int i=0;i<n;i++){
                if(!visited[i] && distance[i] < min){
                    min = distance[i];
                    index = i;
                }
            }
            
            visited[index] = true;
            answer += min;
            count++;
            
            if(count == n){
                break;
            }
            
            for(int i=0;i<n;i++){
                if(!visited[i] && list[index][i] > 0 &&  distance[i] > list[index][i]){
                    distance[i] = list[index][i];
                }
            }
        }
        return answer;
    }
}
```
### **문제 풀이** 
- 모든 정점을 방문하는 최소한의 비용을 구하는 문제이기때문에 최소신장트리를 찾는 방법인 크루스칼 알고리즘 또는 프림 알고리즘으로 해결할 수 있다.

- 프림 알고리즘을 사용하여 문제를 해결하였다.

1. 처음에 0을 시작점으로 0과 연결된 모든 노드들에 대한 방문 길이를 distance에 업데이트 한다. 
2. 그리고 그 다음 차례에 0에서 갈 수 있는 노드들 중 가장 방문 길이가 짧은 노드를 방문 처리한다.
    - 이때 방문처리하면서 해당 노드로 가는 비용을 answer에 더한다.
3. 그리고 그 노드와 연결된 다른 노드들에 대한 방문 길이를 distance 업데이트를 한다.

- 이렇게 1번부터 3번까지 반복하며, 모든 노드들에 대한 방문처리를 완료할 때 answer가 모든 노드를 방문할 때 드는 최소 비용이므로 answer를 return한다.




### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42861