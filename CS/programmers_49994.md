## 방문길이

### ***problem***
게임 캐릭터를 4가지 명령어를 통해 움직이려 합니다. 명령어는 다음과 같습니다.

- U: 위쪽으로 한 칸 가기

- D: 아래쪽으로 한 칸 가기

- R: 오른쪽으로 한 칸 가기

- L: 왼쪽으로 한 칸 가기

캐릭터는 좌표평면의 (0, 0) 위치에서 시작합니다. 좌표평면의 경계는 왼쪽 위(-5, 5), 왼쪽 아래(-5, -5), 오른쪽 위(5, 5), 오른쪽 아래(5, -5)로 이루어져 있습니다.
명령어가 매개변수 dirs로 주어질 때, 게임 캐릭터가 처음 걸어본 길의 길이를 구하여 return 하는 solution 함수를 완성해 주세요.

제한사항
- dirs는 string형으로 주어지며, 'U', 'D', 'R', 'L' 이외에 문자는 주어지지 않습니다.
- dirs의 길이는 500 이하의 자연수입니다.

### ***Solution***

```java
import java.util.ArrayList;

class Solution {
    public int solution(String dirs) {
        int answer = 0;
        ArrayList<String> visit = new ArrayList<String>();
        int[] xy = {5,5};
        for(int i=0;i<dirs.length();i++){
            int[] rXY = {xy[0],xy[1]};
            if(dirs.charAt(i) == 'U' && xy[1]<10)
                xy[1]++;
            else if(dirs.charAt(i) == 'D' && xy[1]>0)
                xy[1]--;
            else if(dirs.charAt(i) == 'L' && xy[0]>0)
                xy[0]--;
            else if(dirs.charAt(i) == 'R' && xy[0]<10)
                xy[0]++;

            String s1 = Integer.toString(rXY[0])+Integer.toString(rXY[1])+Integer.toString(xy[0])+Integer.toString(xy[1]);
            String s2 = Integer.toString(xy[0])+Integer.toString(xy[1])+Integer.toString(rXY[0])+Integer.toString(rXY[1]);

            if(!visit.contains(s1) && !visit.contains(s2) && !s1.equals(s2)){
                visit.add(s1);
                visit.add(s2);
            }
        }
        return visit.size()/2;
    }
}
```
- **visit**은 방문한 경로를 저장하는 ArrayList입니다.
    - 저장형식은 '1100' 좌표 저장 방식입니다.
- 좌표계의 중앙좌표는 여기서 **(5,5)**로 가정합니다.
- **s1**변수는 `현재 x좌표 + 현재 y좌표 + 이동 후 x좌표 + 이동 후 y좌표`입니다.
- **s2**변수는 `이동 후 x좌표 + 이동 후 y좌표 + 현재 x좌표 + 현재 y좌표`입니다.
- 방문한 경로  **visit**에 현재 이동한 경로 **s1,s2**가 존재하는 지 확인합니다.
- 확인 후 없으면 방문한 경로 **visit**에 현재 이동한 경로 **s1,s2**를 추가합니다.
    - s1,s2를 모두 방문한 경로에 추가하는 이유는 s1과 s2모두 같은 선을 지나기 때문입니다.
    -  s1만 추가된다면 나중에 s2의 경우로 이동하면 같은 선을 지나지만 다른 case로 인식되기 때문에 다른 선을 지나는 것으로 취급되기 때문입니다. 
- **visit**은 s1,s2두가지 경우를 모두 포함기 때문에 절반을 나눈 값을 return해줍니다.
### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/49994