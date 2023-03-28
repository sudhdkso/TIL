## **네트워크**


#### ***problem***
네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

#### **제한사항**
- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 `n-1`인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
- computer[i][i]는 항상 1입니다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    public static boolean[] visited;
    public static ArrayList<Integer>[] arr;
    
    public static void dfs(int node){
        visited[node] = true;
        for(int n: arr[node]){
            if(!visited[n]){
                visited[n] = true;
                dfs(n);
            }
        }
        return;
    }
    
    public int solution(int n, int[][] computers) {
        int answer = 0;
        arr = new ArrayList[n];

        for(int i=0;i<n;i++){
            arr[i] = new ArrayList<Integer>();
        }

        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(computers[i][j] == 1 && i != j){
                    arr[i].add(j);
                    arr[j].add(i);
                }
            }
        }

        visited = new boolean[n];
        for(int i=0;i<n;i++){
            if(visited[i] == false){
                dfs(i);
                answer++;
                
            }
        }
        return answer;
    }
}
```
- dfs를 활용하여 하나의 노드에 얼마나 컴퓨터가 연결되어있는지 확인한다.
- arr은 컴퓨터간 연관관계를 저장하는 ArrayList이다.
- 이차원 배열로 들어온 컴퓨터간 연관관계를 그래프 형태로 변경한다.
- 한번 노드가 방문되면 다시한번 노드를 방문할 필요가 없기 때문에 visited를 전역적고 한번만 초기화해준다.
- 노드의 방문여부를 확인하고 방문하지 않았으면 그 노드를 중심으로 dfs를 한다.
- dfs를 할 때 마다 answer를 하나씩 증가시킨다.


### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/43162