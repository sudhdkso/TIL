## **줄 서는 방법**


### ***problem***
n명의 사람이 일렬로 줄을 서고 있습니다. n명의 사람들에게는 각각 1번부터 n번까지 번호가 매겨져 있습니다. n명이 사람을 줄을 서는 방법은 여러가지 방법이 있습니다. 예를 들어서 3명의 사람이 있다면 다음과 같이 6개의 방법이 있습니다.

- [1, 2, 3]
- [1, 3, 2]
- [2, 1, 3]
- [2, 3, 1]
- [3, 1, 2]
- [3, 2, 1]

사람의 수 n과, 자연수 k가 주어질 때, 사람을 나열 하는 방법을 사전 순으로 나열 했을 때, k번째 방법을 return하는 solution 함수를 완성해주세요.

### **제한사항**
- n은 20이하의 자연수 입니다.
- k는 n! 이하의 자연수 입니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    public int[] solution(int n, long k) {
        int[] answer = new int[n];
        
        List<Integer> list = new ArrayList<>();
        
        for(int i=1;i<=n;i++){
            list.add(i);
        }
        
        long f = fac(n);
        
        k = k-1;
        
        for(int i=0;i<n;i++){
            f/=(n-i);
            int num = (int)(k/f);
            answer[i] = list.get(num);
            list.remove(num);
            k%=f;
        }
        return answer;
    }
    
    public static long fac(int n){
        if(n <= 1){
            return n;
        }
        return fac(n-1)*n;
    }
}
```
### **문제 풀이** 

<details>
<summary><b>문제 접근 방법</b></summary>
<div>
처음의 접근 방식은 재귀를 통해서 하나하나씩 차례대로 순열들을 구하면서, 해당 순열의 순서를 count해서 count가 k일 때 지금 순열을 가지고 있는 배열을 정답으로 하는 완전탐색 방식이었다.

하지만 이 방법은 제한사항을 보면 알 수 있듯 k의 최대값은 20!로 	완전탐색으로 구현 시 시간 초과가 날 수 있는 문제였다.

그리고 다른 방법들 중 next_permutation도 있었지만 비슷한 결과를 내었다.

그래서 규칙성을 찾아야한다고 생각을 했고, 문제의 순열들이 `사전 순`이라는 것에 초점을 맞추었다.

문제의 예시에서 n이 3이고 k가 5인 경우 factorial은 3!이기 때문에, 제일 앞의 숫자를 결정하면 뒤의 수는 2!안에서 해결하면 되기 때문에 괜찮을 것이라고 생각했다.

하지만 최악의 경우 19!을 모두 돌아야하기 때문에 이 방법도 실패하였다.

첫자리수도 결정할 수 있다면 다른 자릿수도 결정할 수 있을 것이라고 생각하였다.
</div>
</details>

- 완전 탐색으로 진행하는 경우 시간초과가 나기 때문에, 각 순열들의 가장 첫번째 원소를 구하는 방법으로 문제를 진행하였다.

- 예를들어 n이 3인 경우 최초 사용가능한 원소들을 `[1,2,3]`이다. 그리고 3개의 수로 만들 수 있는 순열의 갯수는 `3!`이며 각 순열들은 앞자리가 결정되면 뒤에 올 수 있는 수는 `2!`개이기 때문에, 2개 단위로 앞자리의 숫자가 달라지게 된다.

- 그리고 인덱스는 0번부터 시작하기 때문에 매개변수로 받은 k에 1을 뺀 값인 k가 된다. 예시에서 k는 5이기 때문에 실제 k값은 4이다.

- n이 3일때 순열들은 2개씩 끊을 수 있기 때문에, `k/2`를 하면 2가 나오고 여기서 2는 [1,2,3]의 인덱스 2인 3을 의미한다. 따라서 순열의 첫번째 자리수는 3이다.

- 이미 사용한 숫자인 3을 제외하고 남은 [1,2]를 가지고 위의 방법 다시 수행한다.
- 이때 경우의 수는 첫자리를 결정지었기 때문에 `2! = (f)`이 되고, k의 경우 k%f한 값이 된다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/12936