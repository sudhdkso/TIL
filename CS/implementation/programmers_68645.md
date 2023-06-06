## **삼각 달팽이**


#### ***problem***
정수 n이 매개변수로 주어집니다. 다음 그림과 같이 밑변의 길이와 높이가 n인 삼각형에서 맨 위 꼭짓점부터 반시계 방향으로 달팽이 채우기를 진행한 후, 첫 행부터 마지막 행까지 모두 순서대로 합친 새로운 배열을 return 하도록 solution 함수를 완성해주세요.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/e1e53b93-dcdf-446f-b47f-e8ec1292a5e0/examples.png" width="500px">

#### **제한사항**
- n은 1 이상 1,000 이하입니다.

### ***Solution***
``` java
import java.util.Arrays;
class Solution {

    private static int[] getResult(int[][] map,int n){
        int[] answer = new int[(n*(n+1))/2];
        int index = 0;
        for(int i = 0;i<n;i++){
            for(int j=0;j<=i;j++){
                answer[index++] = map[i][j]; 
            }
        }
        return answer;
    }
    
    public int[] solution(int n) {
        int[][] map = new int[n][n];
        int x = 0, y = 0;
        int num = 1, count = 0;
        
        if(n == 1){
            return new int[]{1};
        }
        
        while(count < n/2){
            //to down
            while(y+count < n){
                if(map[y][x] != 0) break;
                map[y++][x] = num++;
            }
            y--; x++;
            //to right
            while(x+count < n){
                if(map[y][x] != 0) break;
                map[y][x++] = num++;
            }
            y--; x-=2;
            
            while(x >= 0 && y >= 0 ){
                if(x < 0 || y < 0 || map[y][x] != 0) break;
                map[y--][x--] = num++;
            }
            y+=2; x++;
            count++;
        }

        return getResult(map, n);
    }
}
```
- while루프안에 첫번째 while y값을 증가시키면서 숫자를 채워나가는 반복문이다.
- while루프안의 두번째 while은 x값을 증가시키면서 숫자를 채워나가는 반복문이다.
- 마지막 while은 x,y를 감소시키면서 대각선으로 숫자를 채워나가는 반복문이다.
- 이 3번의 반복문을 한번씩 돌면 삼각 달팽이의 가장 외곽이 모두 채워진다.
- count는 이 큰 while문을 돈 횟수로, 최대 횟수는 n/2이다.
 

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/68645#