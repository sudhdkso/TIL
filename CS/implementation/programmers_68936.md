## **쿼드압축 후 개수 세기**


#### ***problem***
0과 1로 이루어진 2n x 2n 크기의 2차원 정수 배열 arr이 있습니다. 당신은 이 arr을 쿼드 트리와 같은 방식으로 압축하고자 합니다. 구체적인 방식은 다음과 같습니다.

1. 당신이 압축하고자 하는 특정 영역을 S라고 정의합니다.
2. 만약 S 내부에 있는 모든 수가 같은 값이라면, S를 해당 수 하나로 압축시킵니다.
3. 그렇지 않다면, S를 정확히 4개의 균일한 정사각형 영역(입출력 예를 참고해주시기 바랍니다.)으로 쪼갠 뒤, 각 정사각형 영역에 대해 같은 방식의 압축을 시도합니다.

arr이 매개변수로 주어집니다. 위와 같은 방식으로 arr을 압축했을 때, 배열에 최종적으로 남는 0의 개수와 1의 개수를 배열에 담아서 return 하도록 solution 함수를 완성해주세요.




#### **제한사항**
- arr의 행의 개수는 1 이상 1024 이하이며, 2의 거듭 제곱수 형태를 하고 있습니다. 즉, arr의 행의 개수는 1, 2, 4, 8, ..., 1024 중 하나입니다.
    - arr의 각 행의 길이는 arr의 행의 개수와 같습니다. 즉, arr은 정사각형 배열입니다.
    - arr의 각 행에 있는 모든 값은 0 또는 1 입니다.

### ***Solution***
``` java
class Solution {
    static int[] answer = new int[2];
    private static void go(int x,int y,int length,int[][] arr){
        if(isCompressed(x,y,length,arr)){
            if(arr[x][y] == 0){
                answer[0]++;
            }
            else{
                answer[1]++;
            }
            return;
        }

        int next = length/2;
        go(x,y,next,arr);
        go(x+next, y, next,arr);
        go(x,y+next,next,arr);
        go(x+next,y+next,next,arr);
    }
    
    private static boolean isCompressed(int x,int y,int length,int[][] arr){
        int num = arr[x][y];
        for(int i=x;i<x+length;i++){
            for(int j=y;j<y+length;j++){
                if(num != arr[i][j]){
                    return false;
                }
            }
        }
        return true;
    }
    public int[] solution(int[][] arr) {
        int length = arr.length;
        go(0,0,length,arr);
        
        return answer;
    }
}
```
- go()재귀함수를 통해 각 영역을 절반씩 줄여가면서 압축가능한지 확인한다.
    - 압축가능여부를 확인한다.
        -  arr[x][y]가 0이면 0으로 압축하는 것이기에 answer[0]을 증가시킨다.
        - arr[x][y]가 1이면 1으로 압축하는 것이기에 answer[1]을 증가시킨다.
- isCompressed()함수는 압축가능여부를 return하는 함수이다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/68936