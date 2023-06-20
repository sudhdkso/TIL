## **행렬의 곱셈**


#### ***problem***
2차원 행렬 arr1과 arr2를 입력받아, arr1에 arr2를 곱한 결과를 반환하는 함수, solution을 완성해주세요.


#### **제한사항**
- 행렬 arr1, arr2의 행과 열의 길이는 2 이상 100 이하입니다.
- 행렬 arr1, arr2의 원소는 -10 이상 20 이하인 자연수입니다.
- 곱할 수 있는 배열만 주어집니다.

### ***Solution***
``` java
class Solution {
    public static int calcMul(int[][] arr1, int[][] arr2, int x, int y){
        int sum = 0;
        for(int i = 0;i < arr2.length;i++){
            sum += arr1[x][i] * arr2[i][y]; 
        }
        return sum;
    }
    
    public int[][] solution(int[][] arr1, int[][] arr2) {
        int r = arr1.length;
        int c = arr2[0].length;
        int[][] answer = new int[r][c];
        for(int i=0;i<r;i++){
            for(int j=0;j<c;j++){
                answer[i][j] = calcMul(arr1,arr2,i,j);
            }
        }
        return answer;
    }
}
```

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/12949#