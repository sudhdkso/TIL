## **자물쇠와 열쇠**


### ***problem***
고고학자인 "튜브"는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 자물쇠로 잠겨 있었고 문 앞에는 특이한 형태의 열쇠와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

잠겨있는 자물쇠는 격자 한 칸의 크기가 `1 x 1`인 `N x N` 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 `M x M` 크기인 정사각 격자 형태로 되어 있습니다.

자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 

자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

### **제한사항**
- key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
- lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
- M은 항상 N 이하입니다.
- key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
    - 0은 홈 부분, 1은 돌기 부분을 나타냅니다.

### ***Solution***
``` java
import java.util.Arrays;
class Solution {
    static int N,M;
    public boolean solution(int[][] key, int[][] lock) {
        boolean answer = true;
        N = lock.length;
        M = key.length;
        int exLen = N + 2*M -2;
        int[][] extendLock = new int[exLen][exLen];
        for(int i=0;i<N;i++){
            for(int j=0;j<N;j++){
                extendLock[M-1+i][M-1+j] = lock[i][j];
            }
        }
        
        int count = 4;
        for(int i=0;i<count;i++){
            if(check(key, extendLock)){
                return true;
            }
            rotateArray(key);
        }

        return false;
    }
    private static boolean check(int[][] key, int[][] lock){
        int len = N + 2*M -2;
        int count = 0;
        for(int i = 0;i<len-M+1;i++){
            for(int j=0;j<len-M+1;j++){
                int[][] temp = new int[len][len];
                for(int t = 0; t < len; t++) {
                    temp[t] = lock[t].clone();
                }
		
                //TO-DO 
                //temp에 key더하기 
                for(int a=0;a<M;a++){
                    for(int b=0;b<M;b++){
                        temp[i+a][j+b] ^= key[a][b];
                    }
                }
                //TO-DO
                //temp가 모두 1인지 확인하기
                if(isKey(temp)){
                    return true;
                }
            }
        }

        return false;
    }
    
    private static boolean isKey(int[][] lock){
        int len = N+M-1;
        for(int i=M-1;i<len;i++){
            for(int j=M-1;j<len;j++){
                if(lock[i][j] != 1){
                    return false;
                }
            }
        }
        return true;
    }
    
    private static void rotateArray(int[][] arr){
        int n = arr.length;
        int[][] temp = new int[n][n];
        
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                temp[i][j] = arr[n-1-j][i];
            }
        }
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                arr[i][j] = temp[i][j];
            }
        }
    }
    
}
```
- lock배열을 확장시켜서 문제를 해결한다.
    - lock배열의 확장 범위는 `N + 2*M -2`으로 확장한 배열의 가운데 lock배열의 값을 두고, lock배열의 가장 왼쪽이 key배열에 걸칠 수 있는 정도로 확장한다고 가정한다.
    - 왼쪽 뿐만 아니라, 오른쪽, 위, 아래 모두 배열의 범위가 lock배열을 가운데 두었을 때 key배열과 겹칠 수 있는 최소 범위로 지정한다.
- 그리고 확장시킨 lock배열에 (0,0)의 위치 부터 끝까지 key배열의 값과 XOR해준다.
- 그리고 확장시킨 lock배열에서 원래 lock배열의 범위의 값이 모두 1인지 확인한다.

- 위의 행동을 키를 rotateArray()함수를 통해 돌려가며 반복한다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/60059#