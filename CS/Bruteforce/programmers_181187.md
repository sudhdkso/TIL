## **두 원 사이의 정수 쌍**


### ***problem***
x축과 y축으로 이루어진 2차원 직교 좌표계에 중심이 원점인 서로 다른 크기의 원이 두 개 주어집니다. 반지름을 나타내는 두 정수 `r1`, `r2`가 매개변수로 주어질 때, 두 원 사이의 공간에 x좌표와 y좌표가 모두 정수인 점의 개수를 return하도록 solution 함수를 완성해주세요.
※ 각 원 위의 점도 포함하여 셉니다.

#### **제한사항**
- 1 ≤ `r1` < `r2` ≤ 1,000,000

### ***Solution***
``` java
import java.util.*;

class Solution {
    public boolean inCircle(long x,long y, int r1,int r2){
        boolean inR1 = Math.pow(x,2)+Math.pow(y,2) >= Math.pow(r1,2) ? true : false;
        boolean inR2 = Math.pow(x,2)+Math.pow(y,2) <= Math.pow(r2,2) ? true : false;
        if(inR1 && inR2){
            return true;
        }
        return false;
    }
    public long solution(int r1, int r2) {
        long answer = 0;
        long axis = 0;
        for(long i=0;i<=r2;i++){
            long y1 = (long)Math.sqrt(Math.pow(r1,2) - Math.pow(i,2));
            long y2 = (long)Math.sqrt(Math.pow(r2,2) - Math.pow(i,2)); 
            if( i == 0){
                axis += (y2 - y1)+1;
            }
            if(y1 == 0 || y2 == 0){
                axis++;
            }
            
            answer += y2 - y1;
            
            if(inCircle(i,y1,r1,r2) && inCircle(i,y2,r1,r2)){
                answer+=1;
            }
        }
        
        answer = (answer - axis) * 4 + axis *2;
        
        return answer;
    }
}
```
- `r1`,`r2` 조건이 1,000,000까지 되기 때문에 이중 반복문을 사용하여 `(i,j)`조합을 일일 다 확인하면 시간초과된다.
- `inCircle`은 `x`,`y`좌표와 `r1`,`r2` 반지름을 매개변수로 받아 `(x,y)`좌표가 `r1`과 `r2`사이에 존재여부를 boolean타입으로 return해주는 함수이다.

- main내에서 두 원에 대하여 반복문을 돌린다.
    - `y1`은 `i`가 `x`좌표일 때 `r1`내에 존재하는 `y`좌표이다.
    - `y2`는 `i`가 `x`좌표일 때 `r2`내에 존재하는 `y`좌표이다.
    - `axis`는 각 좌표 중 `x`,`y`축에 존재하는 좌표들을 갯수이다.
    - `answer`에 `y2-y1`한 값만큼 더해준다.
    - 그리고 `(i,y1)`과 `(i,y2)`를 `inCircle`함수를 통해 두 원의 사이에 존재하는지 확인한다.
        - 이때 존재하면 `answer`에 `1`을 더해준다.

- 모두 반복한 후에 `answer`는 제1사분면에 존재하는 좌표들만 체크한 값이며 x,y축에 존재하는 좌표들도 포함되기 때문에 `answer - axis`한 후 `*4`를 해주면 각 사분면에 존재하는 두 원사이의 정수 쌍의 갯수를 구할 수 있다.
- 그리고 `axis`로 따로 빼준 값에 `*2` 하여 `answer`에 더해주면 모든 두 원 사이의 정수 쌍을 구할 수 있다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/181187