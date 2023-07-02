## **기지국 설치**


### ***problem***
N개의 아파트가 일렬로 쭉 늘어서 있습니다. 이 중에서 일부 아파트 옥상에는 4g 기지국이 설치되어 있습니다. 기술이 발전해 5g 수요가 높아져 4g 기지국을 5g 기지국으로 바꾸려 합니다. 그런데 5g 기지국은 4g 기지국보다 전달 범위가 좁아, 4g 기지국을 5g 기지국으로 바꾸면 어떤 아파트에는 전파가 도달하지 않습니다.

예를 들어 11개의 아파트가 쭉 늘어서 있고, [4, 11] 번째 아파트 옥상에는 4g 기지국이 설치되어 있습니다. 만약 이 4g 기지국이 전파 도달 거리가 1인 5g 기지국으로 바뀔 경우 모든 아파트에 전파를 전달할 수 없습니다. (전파의 도달 거리가 W일 땐, 기지국이 설치된 아파트를 기준으로 전파를 양쪽으로 W만큼 전달할 수 있습니다.)

- 초기에, 1, 2, 6, 7, 8, 9번째 아파트에는 전파가 전달되지 않습니다.

- 1, 7, 9번째 아파트 옥상에 기지국을 설치할 경우, 모든 아파트에 전파를 전달할 수 있습니다.

- 더 많은 아파트 옥상에 기지국을 설치하면 모든 아파트에 전파를 전달할 수 있습니다.

이때, 우리는 5g 기지국을 최소로 설치하면서 모든 아파트에 전파를 전달하려고 합니다. 위의 예시에선 최소 3개의 아파트 옥상에 기지국을 설치해야 모든 아파트에 전파를 전달할 수 있습니다.

아파트의 개수 N, 현재 기지국이 설치된 아파트의 번호가 담긴 1차원 배열 stations, 전파의 도달 거리 W가 매개변수로 주어질 때, 모든 아파트에 전파를 전달하기 위해 증설해야 할 기지국 개수의 최솟값을 리턴하는 solution 함수를 완성해주세요

### **제한사항**
- N: 200,000,000 이하의 자연수
- stations의 크기: 10,000 이하의 자연수
- stations는 오름차순으로 정렬되어 있고, 배열에 담긴 수는 N보다 같거나 작은 자연수입니다.
- W: 10,000 이하의 자연수

### ***Solution***
``` java
import java.util.*;
class Solution {
    public int solution(int n, int[] stations, int w) {
        int answer = 0;
        int len = stations.length;
        if(stations[0]-w-1 >= 0){
            answer += calcStations(1,stations[0]-w-1,w);
        }
        
        for(int i=1;i<len;i++){
            answer += calcStations(stations[i-1]+w+1,stations[i]-w-1,w);
        }
        
        if(stations[len-1]+w+1<=n){
            answer +=  calcStations(stations[len-1]+w+1,n,w);
        }
        
        return answer;
    }
    
    private static int calcStations(int left, int right, int w){
        int range = (2*w)+1;
        int length = (right-left)+1;
        double d = (double)length/(double)range;
        return ceil(d);
    }
    
    private static int ceil(double d){
        int r = (int)d;
        if(d < 0 || (double)r == d){
            return r;
        }
        return r+1;
    }
}
```
- calcStations은 left와 right를 매개변수로 받아 [left,right]구간 사이에 몇개의 기지국을 설치해야하는 지 계산하여 return하는 함수이다.
- ceil은 double타입의 d를 매개변수로 받아 올림처리 해주는 함수이다.

- solution에서 매개변수로 받은 stations 배열의 값을 통해서 기지국 전파가 도달하지 않는 구간들을 구하면서, 각 구간들에 기지국을 몇개 설치해야하는 지 계산한다.
    - stations[0]의 경우 `[1,stations[0]-w-1]`구간을 체크한다.
        - stations[0]-w-1이 0보다 크면 `[1,stations[0]-w-1]`사이에 전파가 닿지 않는 구간이 있으므로 calcStations을 통해 이 구간내에 몇개의 기지국을 설치해야하는 지 체크한다.
    - stations[len-1]의 경우도 `[stations[len-1]+w+1, n]`구간을 확인해야한다.
        - stations[len-1]+w+1이 n보다 작으면 `[stations[len-1]+w+1, n]`사이에 전파가 닿지 않는 곳이 있으므로 체크해준다.
    - 기지국을 설치해야하는 구간들을 stations[i-1]과 stations[i]사이이기 때문에, stations[i-1]의 오른쪽 전파가 딯지 않는 부분 다음 부터 stations[i]의 왼쪽 전파가 닿지 않는 부분 사이가 기지국을 설치해야하는 구간이라고 할 수 있다.
    - 이를 식으로 나타내면 `[stations[i-1]+w+1, stations[i]-w-1]`이라고 할 수 있다.
    - 주어진 stations들의 모든 `[stations[i-1]+w+1, stations[i]-w-1]`를 각각left와 right로 하여 calcStations을 통해 이 구간에서 설치해야하는 기지국의 갯수를 구할 수 있다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/12979#