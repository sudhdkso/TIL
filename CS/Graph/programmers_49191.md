## **순위**


### ***problem***
n명의 권투선수가 권투 대회에 참여했고 각각 1번부터 n번까지 번호를 받았습니다. 권투 경기는 1대1 방식으로 진행이 되고, 만약 A 선수가 B 선수보다 실력이 좋다면 A 선수는 B 선수를 항상 이깁니다. 심판은 주어진 경기 결과를 가지고 선수들의 순위를 매기려 합니다. 하지만 몇몇 경기 결과를 분실하여 정확하게 순위를 매길 수 없습니다.

선수의 수 n, 경기 결과를 담은 2차원 배열 results가 매개변수로 주어질 때 정확하게 순위를 매길 수 있는 선수의 수를 return 하도록 solution 함수를 작성해주세요.

### **제한 사항**
- 선수의 수는 1명 이상 100명 이하입니다.
- 경기 결과는 1개 이상 4,500개 이하입니다.
- results 배열 각 행 [A, B]는 A 선수가 B 선수를 이겼다는 의미입니다.
- 모든 경기 결과에는 모순이 없습니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    
    private static int[][] count;
    private static ArrayList<Integer>[] list;
    private static boolean[] visited;
    
    public int solution(int n, int[][] results) {
        int answer = 0;
        visited = new boolean[n+1];
        list = new ArrayList[n+1];
        count = new int[n+1][2];
        
        for(int i=1;i<=n;i++){
            list[i] = new ArrayList<>();
        }
        
        for(int i=0;i< results.length;i++){
            list[results[i][0]].add(results[i][1]);
        }
        
        for(int i=1;i<=n;i++){
            Collections.sort(list[i]);
        }
        
        for(int i=1;i<=n;i++){
            visited = new boolean[n+1];
            dfs(i,i);
        }
        
        for(int i=1;i<=n;i++){
            if(count[i][0] + count[i][1] == n-1){
                answer++;
            }
        }

        return answer;
    }
    
    private static void dfs(int node, int start){
        visited[node] = true;
        if(node != start){
            count[node][0]++;
            count[start][1]++;
        }
        for(int n : list[node]){
            if(!visited[n]){
                dfs(n, start);
            }
        }
    }
}
```
### **문제 풀이** 
- dfs를 활용하여 각 노드의 부모노드의 갯수와 자식 노드의 갯수를 구할 수 있다.
- 부모 노드의 갯수와 자식노드의 갯수를 더한 값이 n-1과 같으면 해당 노드는 등수를 알 수 있는 노드이기 때문에 answer를 증가시킨다.



### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/49191