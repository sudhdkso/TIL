## **N-Quuen**


### ***problem***
가로, 세로 길이가 n인 정사각형으로된 체스판이 있습니다. 체스판 위의 n개의 퀸이 서로를 공격할 수 없도록 배치하고 싶습니다.

예를 들어서 n이 4인경우 다음과 같이 퀸을 배치하면 n개의 퀸은 서로를 한번에 공격 할 수 없습니다.

체스판의 가로 세로의 세로의 길이 n이 매개변수로 주어질 때, n개의 퀸이 조건에 만족 하도록 배치할 수 있는 방법의 수를 return하는 solution함수를 완성해주세요.
#### **제한사항**
- 퀸(Queen)은 가로, 세로, 대각선으로 이동할 수 있습니다.
- n은 12이하의 자연수 입니다.


### ***Solution***
``` java
class Solution {
    public static int[] arr;
    public static int count = 0;
    public static void setQueen(int index, int n ){
        if(index == n){
            count++;
            return;
        }
        
        for(int i=0;i<n;i++){
            arr[index] = i;
            if(!check(index)){
                setQueen(index+1,n);
            }    
        }
    }
    
    public static boolean check(int b){
        for(int a=0; a<b;a++){
            if((Math.abs(a-b) == Math.abs(arr[a]-arr[b]))) return true;   
        }
        return false;
    }
    
    public int solution(int n) {
        int answer = 0;
        arr = new int[n];
        setQueen(0,n);
        
        return count;
    }
}
```

- n-queen은 대표적인 백트래킹 문제이다.
- queen을 한자리씩 놓으면서 그다음 queen이 놓일 자리고 있으면 queen을 놓고, 놓을 자리가 없으면 return해준다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/12952