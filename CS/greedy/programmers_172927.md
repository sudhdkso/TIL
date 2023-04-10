## **광물 캐기**


#### ***problem***
마인은 곡괭이로 광산에서 광석을 캐려고 합니다. 마인은 다이아몬드 곡괭이, 철 곡괭이, 돌 곡괭이를 각각 0개에서 5개까지 가지고 있으며, 곡괭이로 광물을 캘 때는 피로도가 소모됩니다. 각 곡괭이로 광물을 캘 때의 피로도는 아래 표와 같습니다.

images

예를 들어, 철 곡괭이는 다이아몬드를 캘 때 피로도 5가 소모되며, 철과 돌을 캘때는 피로도가 1씩 소모됩니다. 각 곡괭이는 종류에 상관없이 광물 5개를 캔 후에는 더 이상 사용할 수 없습니다.

마인은 다음과 같은 규칙을 지키면서 최소한의 피로도로 광물을 캐려고 합니다.

- 사용할 수 있는 곡괭이중 아무거나 하나를 선택해 광물을 캡니다.
- 한 번 사용하기 시작한 곡괭이는 사용할 수 없을 때까지 사용합니다.
- 광물은 주어진 순서대로만 캘 수 있습니다.
- 광산에 있는 모든 광물을 캐거나, 더 사용할 곡괭이가 없을 때까지 광물을 캡니다.

즉, 곡괭이를 하나 선택해서 광물 5개를 연속으로 캐고, 다음 곡괭이를 선택해서 광물 5개를 연속으로 캐는 과정을 반복하며, 더 사용할 곡괭이가 없거나 광산에 있는 모든 광물을 캘 때까지 과정을 반복하면 됩니다.

마인이 갖고 있는 곡괭이의 개수를 나타내는 정수 배열 `picks`와 광물들의 순서를 나타내는 문자열 배열 `minerals`가 매개변수로 주어질 때, 마인이 작업을 끝내기까지 필요한 최소한의 피로도를 return 하는 solution 함수를 완성해주세요.
#### **제한사항**
- `picks`는 [dia, iron, stone]과 같은 구조로 이루어져 있습니다.
    - 0 ≤ dia, iron, stone ≤ 5
    - dia는 다이아몬드 곡괭이의 수를 의미합니다.
    - iron은 철 곡괭이의 수를 의미합니다.
    - stone은 돌 곡괭이의 수를 의미합니다.
    - 곡괭이는 최소 1개 이상 가지고 있습니다.
- 5 ≤ `minerals`의 길이 ≤ 50
    - `minerals`는 다음 3개의 문자열로 이루어져 있으며 각각의 의미는 다음과 같습니다.
    - diamond : 다이아몬드
    - iron : 철
    - stone : 돌


### ***Solution***
``` java
import java.util.*;

class Solution {
    public static int solution(int[] picks, String[] minerals) {
        int answer = 0;
        int pickNum = 0;

        for(int i=0;i<picks.length;i++){
            pickNum+=picks[i];
        }

        int[][] fatigues = calcFatigue(minerals, pickNum);

        int length = fatigues.length;

        Arrays.sort(fatigues, (o1,o2) -> {
            if(o1[2] == o2[2]){
                if(o1[1] == o2[1]){
                    return o2[0] - o1[0];
                }
                return o2[1]-o1[1];
            }
            return o2[2]-o1[2];
        });

        for(int i=0;i<length;i++){
            if(picks[0] > 0){
                answer+= fatigues[i][0];
                picks[0]--;
            }
            else if(picks[1] > 0){
                answer+= fatigues[i][1];
                picks[1]--;
            }
            else if(picks[2] > 0){
                answer+= fatigues[i][2];
                picks[2]--;
            }
            else {
                break;
            }
        }


        return answer;
    }

    public static int[][] calcFatigue(String[] minerals, int pickNum){
        int length = minerals.length > pickNum*5 ? pickNum*5 : minerals.length;
        int size = (length/5)+ (length%5 > 0 ? 1 : 0);
        int[][] fatigues = new int[size][3];

        for(int i=0;i<length;i++){
            if(minerals[i].equals("diamond")){
                fatigues[i/5][0] += 1;
                fatigues[i/5][1] += 5;
                fatigues[i/5][2] += 25;
            }
            else if(minerals[i].equals("iron")){
                fatigues[i/5][0] += 1;
                fatigues[i/5][1] += 1;
                fatigues[i/5][2] += 5;
            }
            else if(minerals[i].equals("stone")){
                fatigues[i/5][0] += 1;
                fatigues[i/5][1] += 1;
                fatigues[i/5][2] += 1;
            }
        }
        return fatigues;
    }
}
```
- fatigues는 5개씩 광물을 캘때 필요한 피로도를 저장하는 int형 2차원 배열이다.
    - fatigues[i][0]은 [i , (i*5)-1]까지 광물을 다이아몬드 곡괭이로 캘 때 필요한 피로도이다.
    - fatigues[i][1]은 [i , (i*5)-1]까지 광물을 철 곡괭이로 캘 때 필요한 피로도이다.
    - fatigues[i][2]은 [i , (i*5)-1]까지 광물을 돌 곡괭이로 캘 때 필요한 피로도이다.
- calcFatigues함수는 minerals를 받아 각각의 곡괭이로 광물을 캘 때의 피로도를 계산하는 함수이다.
    - 이때 광물의 갯수가 곡괭이로 모두 캘 수 없는 경우도 존재한다.
    - 그래서 곡괭이로 pickNum*5와 
    minerals.length를 비교하여 minerals.length가 더 길면 pickNum*5까지만 피로도를 계산한다.
    - 그게 아니면 minerals.length까지 계산한다.
        - 광물을 순서대로 캐야하기 때문에 곡괭이로 캐지 못하는 광물들은 slice해줘야 한다.
- calcFatigues를 통해 계산한 fatigues를 돌곡괭이로 캘 때 피로도가 많이 드는 순으로 정렬을 한다.
- 그리고 fatigues를 순서대로 반복하며 곡괭이를 하나하나 사용하고 드는 피로도를 answer에 더해준다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/172927#