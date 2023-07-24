## **숫자의 표현**


### ***problem***
Finn은 요즘 수학공부에 빠져 있습니다. 수학 공부를 하던 Finn은 자연수 n을 연속한 자연수들로 표현 하는 방법이 여러개라는 사실을 알게 되었습니다. 예를들어 15는 다음과 같이 4가지로 표현 할 수 있습니다.

- 1 + 2 + 3 + 4 + 5 = 15
- 4 + 5 + 6 = 15
- 7 + 8 = 15
- 15 = 15

자연수 n이 매개변수로 주어질 때, 연속된 자연수들로 n을 표현하는 방법의 수를 return하는 solution를 완성해주세요.


#### **제한사항**
- n은 10,000 이하의 자연수 입니다.

### ***Solution***
### 1. 완전탐색 풀이
``` java
class Solution {
    public int solution(int n) {
        int answer = 0;
        for(int i=1;i<=n;i++){
            int sum = 0;
            for(int j=i;j<=n;j++){
                sum += j;
                if(sum == n){
                    answer++;
                }
                if(sum > n){
                    break;
                }
                
            }
        }
        return answer;
    }
}
```
### **문제 풀이** 
- n이 10000이기 때문에 어느정도 완전탐색으로도 문제를 풀 수 있을 것이라고 생각하였다.  이중 반복문이기 때문에 10000*10000은 1억인데 최악의 경우라도 10000개까지는 반복문을 돌 가능성이 없기 때문에 충분히 완전탐색으로도 풀 수 있는 문제이다.

- 완전 탐색을 통해 i부터 n까지 더하다가 , 더한 값이 n을 넘어가면, i를 증가시키고 다시 확인하는 방법으로 문제를 해결할 수 있다.
    - 이때 n을 넘어가는 경우 바로 반복문을 탈출하지 않으면, 최악의 경우 시간초과가 발생할 수 있다.
### 2. 구간합 풀이
``` java
import java.util.*;
class Solution {
    public int solution(int n) {
        int answer = 0;
        int[] prefix = calcPriSum(n);
        
        int left = 0, right = 0;
        while(left<= right && right <= n){
            int sum = prefix[right]-prefix[left];
            if(sum  == n){
                answer++;
                left++;
            }
            else if(sum < n){
                right++;
            }
            else if(sum > n){
                left++;
            }
            
        }
        return answer;
    }
    
    private static int[] calcPriSum(int n){
        int[] arr = new int[n+1];
        arr[0] = 0;
        for(int i=1;i<=n;i++){
            arr[i] = i + arr[i-1];
        }
        return arr;
    }
}
```
### **문제 풀이** 
- 연속된 숫자의 합으로 n을 표현해야하기 때문에 구간합 방법을 사용할 수 있다.

- 1부터 n까지의 구간합을 모두 구해놓은 다음, left와 right를 통해 n을 만드는 구간의 갯수를 확인한다.


- 1번의 완전탐색 방법과 2번의 구간합 모두 정답이지만, 효율성 테스트를 보면 구간합을 사용하는 경우가 속도가 훨씬 빠르다는 것을 알 수 있다.
    
### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/12924