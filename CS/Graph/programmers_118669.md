## **등산코스 정하기**


### ***problem***
XX산은 `n`개의 지점으로 이루어져 있습니다. 각 지점은 1부터 `n`까지 번호가 붙어있으며, 출입구, 쉼터, 혹은 산봉우리입니다. 각 지점은 양방향 통행이 가능한 등산로로 연결되어 있으며, 서로 다른 지점을 이동할 때 이 등산로를 이용해야 합니다. 이때, 등산로별로 이동하는데 일정 시간이 소요됩니다.

등산코스는 방문할 지점 번호들을 순서대로 나열하여 표현할 수 있습니다.
예를 들어 `1-2-3-2-1` 으로 표현하는 등산코스는 1번지점에서 출발하여 2번, 3번, 2번, 1번 지점을 순서대로 방문한다는 뜻입니다.
등산코스를 따라 이동하는 중 쉼터 혹은 산봉우리를 방문할 때마다 휴식을 취할 수 있으며, 휴식 없이 이동해야 하는 시간 중 가장 긴 시간을 해당 등산코스의 `intensity`라고 부르기로 합니다.

당신은 XX산의 출입구 중 한 곳에서 출발하여 산봉우리 중 한 곳만 방문한 뒤 다시 원래의 출입구로 돌아오는 등산코스를 정하려고 합니다. 다시 말해, 등산코스에서 출입구는 **처음과 끝에 한 번씩**, 산봉우리는 **한 번**만 포함되어야 합니다.
당신은 이러한 규칙을 지키면서 `intensity`가 최소가 되도록 등산코스를 정하려고 합니다.

다음은 XX산의 지점과 등산로를 그림으로 표현한 예시입니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d1764091-629a-414b-9f77-e2ff1b38c6e0/desc1-1.PNG" title="" alt="desc1-1.PNG">

- 위 그림에서 원에 적힌 숫자는 지점의 번호를 나타내며, 1, 3번 지점에 출입구, 5번 지점에 산봉우리가 있습니다. 각 선분은 등산로를 나타내며, 각 선분에 적힌 수는 이동 시간을 나타냅니다. 예를 들어 1번 지점에서 2번 지점으로 이동할 때는 3시간이 소요됩니다.

위의 예시에서 `1-2-5-4-3` 과 같은 등산코스는 처음 출발한 원래의 출입구로 돌아오지 않기 때문에 잘못된 등산코스입니다. 또한 `1-2-5-6-4-3-2-1` 과 같은 등산코스는 코스의 처음과 끝 외에 3번 출입구를 방문하기 때문에 잘못된 등산코스입니다.

등산코스를 `3-2-5-4-3` 과 같이 정했을 때의 이동경로를 그림으로 나타내면 아래와 같습니다.
<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/ae2b6ccd-290b-4074-aebe-028c13dc4cbe/desc1-2.PNG" title="" alt="desc1-2.PNG">

이때, 휴식 없이 이동해야 하는 시간 중 가장 긴 시간은 5시간입니다. 따라서 이 등산코스의 `intensity`는 5입니다.

등산코스를 `1-2-4-5-6-4-2-1` 과 같이 정했을 때의 이동경로를 그림으로 나타내면 아래와 같습니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/165bcca3-ee06-46b4-95f8-7c3cedd2cb42/desc1-3.PNG" title="" alt="desc1-3.PNG">

이때, 휴식 없이 이동해야 하는 시간 중 가장 긴 시간은 3시간입니다. 따라서 이 등산코스의 `intensity`는 3이며, 이 보다 `intensity`가 낮은 등산코스는 없습니다.

XX산의 지점 수 `n`, 각 등산로의 정보를 담은 2차원 정수 배열 `paths`, 출입구들의 번호가 담긴 정수 배열 `gates`, 산봉우리들의 번호가 담긴 정수 배열 `summits`가 매개변수로 주어집니다. 이때, `intensity`가 최소가 되는 등산코스에 포함된 산봉우리 번호와 `intensity`의 최솟값을 차례대로 정수 배열에 담아 return 하도록 solution 함수를 완성해주세요. `intensity`가 최소가 되는 등산코스가 여러 개라면 그중 산봉우리의 번호가 가장 낮은 등산코스를 선택합니다.

### **제한 사항**
- 2 ≤ `n` ≤ 50,000
- `n` - 1 ≤ `paths`의 길이 ≤ 200,000
- `paths`의 원소는 `[i, j, w]` 형태입니다.
    - `i`번 지점과 `j`번 지점을 연결하는 등산로가 있다는 뜻입니다.
    - `w`는 두 지점 사이를 이동하는 데 걸리는 시간입니다.
    - 1 ≤ `i` < `j` ≤ `n`
    - 1 ≤ `w` ≤ 10,000,000
    - 서로 다른 두 지점을 직접 연결하는 등산로는 최대 1개입니다.
- 1 ≤ `gates`의 길이 ≤ `n`
    - 1 ≤ `gates`의 원소 ≤ `n`
    - `gates`의 원소는 해당 지점이 출입구임을 나타냅니다.
- 1 ≤ `summits`의 길이 ≤ `n`
    - 1 ≤ `summits`의 원소 ≤ `n`
    - `summits`의 원소는 해당 지점이 산봉우리임을 나타냅니다.
- 출입구이면서 동시에 산봉우리인 지점은 없습니다.
- `gates`와 `summits`에 등장하지 않은 지점은 모두 쉼터입니다.
- 임의의 두 지점 사이에 이동 가능한 경로가 항상 존재합니다.
- return 하는 배열은 `[산봉우리의 번호, intensity의 최솟값]` 순서여야 합니다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    private static class Node implements Comparable<Node>{
        int index,cost;
        public Node(int index, int cost){
            this.index = index;
            this.cost = cost;
        }
        
        @Override
        public int compareTo(Node e){
            return Integer.compare(this.cost, e.cost);
        }
    } 
    
    static List<Node>[] list;    
    static int INF = Integer.MAX_VALUE;
    static HashSet<Integer> gateSet = new HashSet<>();
    static HashSet<Integer> sumSet = new HashSet<>();
    static int index = 0;
    
    public int[] solution(int n, int[][] paths, int[] gates, int[] summits) {
        int[] answer = new int[2];
        
        int len = paths.length;
        
        list = new List[n+1];
        
        for(int i=1;i<=n;i++){
            list[i] = new ArrayList<>();
        }
        
        for(int i=0;i<len;i++){
            int a = paths[i][0];
            int b = paths[i][1];
            int c = paths[i][2];
            
            list[a].add(new Node(b,c));
            list[b].add(new Node(a,c));
        }
        
        Arrays.sort(gates);

        for(int i=0;i<gates.length;i++){
            gateSet.add(gates[i]);
        }

        Arrays.sort(summits);

        for(int i=0;i<summits.length;i++){
            sumSet.add(summits[i]);
        }
        
        int low = 1, high = 10_000_000;
        int min = INF;
        while(low <= high){
            int mid = (low+high)/2;
            if(go(n, gates, mid)){
                min = Math.min(min, mid);
                high = mid - 1;
            }
            else{
                low = mid + 1;
            }
        }
        answer[0] = index;
        answer[1] = min;
        return answer;
    }
    
    public static boolean go(int n, int[] gates, int threshold){
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        boolean[] visited = new boolean[n+1];
        int min = INF;
        for(int i=0;i<gates.length;i++){
            pq.offer(gates[i]);    
        }
        
        
        while(!pq.isEmpty()){
            int now = pq.poll();
            
            if(visited[now]) continue;
            visited[now] = true;
            
            if(sumSet.contains(now)){
                min = Math.min(min, now);
                continue;
            }

            for(Node next : list[now]){
                if(visited[next.index] || next.cost > threshold || gateSet.contains(next.index)) continue;
                pq.offer(next.index);
            }
        }
        
        if(min != INF){
            index = min;
            return true;
        }
        return false;
    } 
}
```
### **문제 풀이** 
- 이 문제는 bfs와 이분탐색을 통해서 구현하였다.
- 문제에서 각 출입구는 등산을 시작할때와 종료할때만 방문가능하며, 산봉우리는 한번만 방문할 수 있다. 따라서 크게 보면 `시작-산봉우리-종료`의 지점을 방문하는 것 처럼 보인다.
- 하지만 문제에서는 시작한 출입구에서 원래 출입구로 돌아오는 방식이다. 그래서 `시작-산봉우리-종료`이 아닌 시작 출입구에서 어떤 산봉우리를 방문할 때 최소한의 intensity를 가지는지 구하면 된다.

- 이때 산봉우리는 한번만 방문하고, 중간에 다른 출입구를 방문할 수 없기 때문에 해당 노드들을 heap을 통해서 관리한다.
    - 각 출입구와 산봉우리들에 대한 각각의 HashSet을 생성하여 set에 출입구와 산봉우리의 노드들을 저장하였다.

- 그리고 이분탐색을 통해서 해당 threshold에 산봉우리를 방문가능한지 확인하였다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/118669#