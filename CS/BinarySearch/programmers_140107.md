## **점 찍기**


### ***problem***
좌표평면을 좋아하는 진수는 x축과 y축이 직교하는 2차원 좌표평면에 점을 찍으면서 놀고 있습니다. 진수는 두 양의 정수 `k`, `d`가 주어질 때 다음과 같이 점을 찍으려 합니다.

- 원점(0, 0)으로부터 x축 방향으로 `a*k`(a = 0, 1, 2, 3 ...), y축 방향으로 `b*k`(b = 0, 1, 2, 3 ...)만큼 떨어진 위치에 점을 찍습니다.
- 원점과 거리가 `d`를 넘는 위치에는 점을 찍지 않습니다.
예를 들어, `k`가 2, `d`가 4인 경우에는 (0, 0), (0, 2), (0, 4), (2, 0), (2, 2), (4, 0) 위치에 점을 찍어 총 6개의 점을 찍습니다.

정수 `k`와 원점과의 거리를 나타내는 정수 `d`가 주어졌을 때, 점이 총 몇 개 찍히는지 return 하는 solution 함수를 완성하세요.


#### **제한사항**
- 1 ≤ `k` ≤ 1,000,000
- 1 ≤ `d` ≤ 1,000,000



### ***Solution***
``` java
import java.util.Arrays;

class Solution {
    static long[] coordinateList;
    private static void setCoordinateList(int k, int d){
        int size = (d/k)+1;
        coordinateList = new long[size];
        for(int i=0;i<size;i++){
            coordinateList[i] = (long)k * i;
        }
    }
    private static long binarySearch(long x, long k, long d){
        long start = 0, end = coordinateList.length-1;
        long maxY = 0;
        while(start <= end){
            long mid = (start+end)/2;
            long y = coordinateList[(int)mid];
            if(Math.sqrt((x*x)+(y*y)) <= d){
                maxY = y;
                start = mid+1; 
            }
            else{
                end = mid-1;
            }
        }

        return (maxY/k)+1;
    }
    public long solution(int k, int d) {
        long answer = 0;
        setCoordinateList(k,d);
        for(long coordi : coordinateList){
            answer += binarySearch(coordi,k, d);
        }
        return answer;
    }
}
```
- setCoordinateList()함수는 입력받은 k를 이용하여 정답이 될 수 있는 좌표들만 coorinateList라는 long형 배열에 넣어주는 함수이다.

- binarySearch()함수는 이진탐색을 통해 입력 받은 x좌표일 때 올 수 있는 가장 큰 Y값을 찾아서 x좌표일 때 점을 찍을 수 있는 갯수를 반환하는 함수이다.
    1. binarySearch()함수에서 이진 탐색을 통해서 mid값을 (start+end)/2로 설정한다.
    2. 그리고 y값을 coordinateList[mid]값으로 설정한다.
    3. **Math.sqrt((x\*x)+(y\*y))** 값이 d값보다 작으면 최대Y값이 `mid ~ end`사이에 있으므로 start를 mid+1로 설정한다. 그리고 maxY값을 y로 변경한다.
    4. **Math.sqrt((x\*x)+(y\*y))** 값이 d보다 크면 최대Y값이 start~mid사이에 있으므로 end = mid-1로 한다.
    5.이진탐색을 끝난 후 maxY값에 k를 나눈 후 1을 더한 값을 반환한다.
        - 좌표가 x일 때 올 수 있는 y좌표의 갯수


### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/140107