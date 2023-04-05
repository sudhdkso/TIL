## **피로도**


#### ***problem***
XX게임에는 피로도 시스템(0 이상의 정수로 표현합니다)이 있으며, 일정 피로도를 사용해서 던전을 탐험할 수 있습니다. 이때, 각 던전마다 탐험을 시작하기 위해 필요한 "최소 필요 피로도"와 던전 탐험을 마쳤을 때 소모되는 "소모 피로도"가 있습니다. "최소 필요 피로도"는 해당 던전을 탐험하기 위해 가지고 있어야 하는 최소한의 피로도를 나타내며, "소모 피로도"는 던전을 탐험한 후 소모되는 피로도를 나타냅니다. 예를 들어 "최소 필요 피로도"가 80, "소모 피로도"가 20인 던전을 탐험하기 위해서는 유저의 현재 남은 피로도는 80 이상 이어야 하며, 던전을 탐험한 후에는 피로도 20이 소모됩니다.

이 게임에는 하루에 한 번씩 탐험할 수 있는 던전이 여러개 있는데, 한 유저가 오늘 이 던전들을 최대한 많이 탐험하려 합니다. 유저의 현재 피로도 k와 각 던전별 "최소 필요 피로도", "소모 피로도"가 담긴 2차원 배열 dungeons 가 매개변수로 주어질 때, 유저가 탐험할수 있는 최대 던전 수를 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- k는 1 이상 5,000 이하인 자연수입니다.
- dungeons의 세로(행) 길이(즉, 던전의 개수)는 1 이상 8 이하입니다.
    - dungeons의 가로(열) 길이는 2 입니다.
    - dungeons의 각 행은 각 던전의 ["최소 필요 피로도", "소모 피로도"] 입니다.
    - "최소 필요 피로도"는 항상 "소모 피로도"보다 크거나 같습니다.
    - "최소 필요 피로도"와 "소모 피로도"는 1 이상 1,000 이하인 자연수입니다.
    - 서로 다른 던전의 ["최소 필요 피로도", "소모 피로도"]가 서로 같을 수 있습니다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    public static int max = Integer.MIN_VALUE;
    public static ArrayList<Integer> arrayList = new ArrayList<>();

    public static int solution(int k, int[][] dungeons) {
        int length = dungeons.length;
        boolean[] visited = new boolean[length];
        
        dfs(k,0, dungeons, visited);

        Collections.sort(arrayList, Collections.reverseOrder());

        return arrayList.get(0);
    }

    public static void dfs(int k, int count, int[][] dungeons, boolean[] visited){

        int length = dungeons.length;

        for(int i=0; i<length;i++){
            if(!visited[i] && k >= dungeons[i][0]){
                visited[i] = true;
                dfs(k-dungeons[i][1], count+1, dungeons, visited);
                visited[i] = false;
            }
        }
        arrayList.add(count);
    }
}
```
- visited는 던전 i에 방문 여부를 저장하는 boolean타입의 일차원 배열이다.
- dfs는 k, count, 던전 배열, visited 배열을 매개변수로 받는 함수이다.
    - k는 문제에서 주어지는 현재 피로도(남은 피로도)이다.
    - count는 방문한 던전의 개수이다.
    - dungeons는 문제에서 주어지는 던전들에 관한 정보를 담고 있는 2차원 배열이다.
    - visited는 던전 i에 방문 여부를 저장하는 boolean타입의 일차원 배열이다.
- dfs 함수내에서 모든 던전을 방문하며 방문 가능한 경우의 방문한 던전의 갯수를 arrayList에 넣는다.
    - 던전을 방문하는 경우는 현재 던전을 방문하지 않으며, 현재 피로도가 던전의 필요 피로도보다 크거나 같을 때 해당 던전을 방문할 수 있다.
    - 해당 던전을 방문한 경우 visited[i]를 true로 하고, 남은 피로도로 다시 재귀 호출한다.
- 모든 경우에 대해서 dfs한 후 arrayList를 내림차순으로 정렬하여 가장 첫번째에 있는 count가 최대로 방문 가능한 던전의 개수이다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/87946