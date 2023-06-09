## **전력망을 둘로 나누기**


### ***problem***
n개의 송전탑이 전선을 통해 하나의 트리 형태로 연결되어 있습니다. 당신은 이 전선들 중 하나를 끊어서 현재의 전력망 네트워크를 2개로 분할하려고 합니다. 이때, 두 전력망이 갖게 되는 송전탑의 개수를 최대한 비슷하게 맞추고자 합니다.

송전탑의 개수 n, 그리고 전선 정보 wires가 매개변수로 주어집니다. 전선들 중 하나를 끊어서 송전탑 개수가 가능한 비슷하도록 두 전력망으로 나누었을 때, 두 전력망이 가지고 있는 송전탑 개수의 차이(절대값)를 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- n은 2 이상 100 이하인 자연수입니다.
- wires는 길이가 `n-1`인 정수형 2차원 배열입니다.
    - wires의 각 원소는 [v1, v2] 2개의 자연수로 이루어져 있으며, 이는 전력망의 v1번 송전탑과 v2번 송전탑이 전선으로 연결되어 있다는 것을 의미합니다.
    - 1 ≤ v1 < v2 ≤ n 입니다.
전력망 네트워크가 하나의 트리 형태가 아닌 경우는 입력으로 주어지지 않습니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    static List<Integer>[] list;
    static boolean[] visited;
    static int count = 0;
    public int solution(int n, int[][] wires) {
        int answer = Integer.MAX_VALUE;
        list = new ArrayList[n+1];
        for(int i=1;i<=n;i++){
            list[i] = new ArrayList<>();
        }
        
        for(int i=0;i<wires.length;i++){
            int a = wires[i][0];
            int b = wires[i][1];
            list[a].add(b);
            list[b].add(a);
        }
        
        for(int i=0;i<wires.length;i++){
            visited = new boolean[n+1];
            int a = wires[i][0];
            int b = wires[i][1];
            count = 0;
            dfs(a,b);
            answer = Math.min(answer, (Math.abs((n-count)-count)));
        }
        return answer;
    }
    
    private static void dfs(int node, int dst){
        visited[node] = true;
        if(node == dst){
            return;
        }
        for(int i : list[node]){
            if(!visited[i]){
                visited[i] = true;
                count++;
                dfs(i,dst);
                visited[i] = false;
            }
        }
    }
}
```
- wires을 List에 넣어 노드들의 연결 상태를 저장한다.
- 그 이후 wires[i][0]을 시작 노드로 wires[i][1]을 목표지로 설정하고 dfs를 통해 주어진 wires정보에 대해서 모두 탐색한다.
    - 이때 한 wires[i]에 대해서 dfs를 통해 탐색할 때 count를 증가시킨다.
        - count는 wires[i][0]부터 dfs를 돌 때 wires[i][1]과 wires[i][0]이 끊겨있을 때 탐색 가능한 node의 개수이다.
    - count는 wires[i][0]을 시작노드로 탐색할 때 탐색 가능한 노드의 개수이고, n-count는 나머지 노드의 개수이다.
    - 따라서 wires[i]가 연결되어있지 않아 전력망이 둘로 나눠질 때 연결되어있는 노드는 각각 count, n-count개로 두개의 차이는 (n-count)-count이다.
    - 그리고 Math.min을 활용해서 제일 차이가 작은 값을 구한다.
### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/86971