## **가장 먼 노드**


#### ***problem***
n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.

#### **제한사항**
- 노드의 개수 n은 2 이상 20,000 이하입니다.
- 간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
- vertex 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    static List<Integer>[] list;
    static int[] distance;
    
    public int solution(int n, int[][] edge) {
        int answer = 0;
        list = new ArrayList[n+1];
        distance = new int[n+1];
        
        for(int i=0;i<=n;i++){
            list[i] = new ArrayList<>();
        }
        
        int len = edge.length;
        
        for(int i=0;i<len;i++){
            int a = edge[i][0];
            int b = edge[i][1];
            list[a].add(b);
            list[b].add(a);
        }
        
        for(int i=0;i<=n;i++){
            Collections.sort(list[i]);
        }
        
        bfs(1);
        
        Arrays.sort(distance);
        
        int max = distance[n];
        
        for(int d : distance){
            if(max == d){
                answer++;
            }
        }
        
        return answer;
    }
    
    private static void bfs(int start){
        Queue<Integer> q = new LinkedList<>();
        distance[start] = 1;
        q.offer(start);
        while(!q.isEmpty()){
            int node = q.poll();
            for(int n : list[node]){
                if(distance[n] == 0){
                    distance[n] = distance[node]+1;
                    q.offer(n);
                }
            }
        }
    }
}
```
- 가장 먼 노드를 찾기 위해서 bfs를 활용한다.
- distance를 통해서 각 노드들의 거리가 1로부터 얼마나 떨어져있는지 저장한다.
- 그리고 bfs를 통해 distance를 모두 계산한 후 distance를 정렬한다.
- distance중 가장 큰 값을 max로 지정하여 distance에 max와 같은 값이 몇개인지 answer에 저장한다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/49189