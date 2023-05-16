## **정수 삼각형**


### ***problem***
철수와 영희는 선생님으로부터 숫자가 하나씩 적힌 카드들을 절반씩 나눠서 가진 후, 다음 두 조건 중 하나를 만족하는 가장 큰 양의 정수 a의 값을 구하려고 합니다.

1. 철수가 가진 카드들에 적힌 모든 숫자를 나눌 수 있고 영희가 가진 카드들에 적힌 모든 숫자들 중 하나도 나눌 수 없는 양의 정수 a
2. 영희가 가진 카드들에 적힌 모든 숫자를 나눌 수 있고, 철수가 가진 카드들에 적힌 모든 숫자들 중 하나도 나눌 수 없는 양의 정수 a


예를 들어, 카드들에 10, 5, 20, 17이 적혀 있는 경우에 대해 생각해 봅시다. 만약, 철수가 [10, 17]이 적힌 카드를 갖고, 영희가 [5, 20]이 적힌 카드를 갖는다면 두 조건 중 하나를 만족하는 양의 정수 a는 존재하지 않습니다. 하지만, 철수가 [10, 20]이 적힌 카드를 갖고, 영희가 [5, 17]이 적힌 카드를 갖는다면, 철수가 가진 카드들의 숫자는 모두 10으로 나눌 수 있고, 영희가 가진 카드들의 숫자는 모두 10으로 나눌 수 없습니다. 따라서 철수와 영희는 각각 [10, 20]이 적힌 카드, [5, 17]이 적힌 카드로 나눠 가졌다면 조건에 해당하는 양의 정수 a는 10이 됩니다.

철수가 가진 카드에 적힌 숫자들을 나타내는 정수 배열 arrayA와 영희가 가진 카드에 적힌 숫자들을 나타내는 정수 배열 arrayB가 주어졌을 때, 주어진 조건을 만족하는 가장 큰 양의 정수 a를 return하도록 solution 함수를 완성해 주세요. 만약, 조건을 만족하는 a가 없다면, 0을 return 해 주세요.
#### **제한사항**
- 1 ≤ `arrayA`의 길이 = `arrayB`의 길이 ≤ 500,000
- 1 ≤ `arrayA`의 원소, `arrayB`의 원소 ≤ 100,000,000
- `arrayA`와 `arrayB`에는 중복된 원소가 있을 수 있습니다.
### ***Solution***
``` java
class Solution {
    public int gcd(int a,int b){
        if( b ==0) return a;
        return gcd(b,a%b);
    }

    public boolean checkDivision(int num,int[] arr){
        int len = arr.length;
        for(int i=0;i<len;i++){
            if(arr[i]%num == 0) return true;
        }
        return false;
    }
    public int solution(int[] arrayA, int[] arrayB) {
        int answer = 0;
        int a = arrayA[0];
        int b = arrayB[0];
        int len = arrayA.length;
        for(int i=1;i<len;i++){
            a = gcd(a,arrayA[i]);
        }
        
        for(int i=1;i<len;i++){
            b = gcd(b,arrayB[i]);
        }
        
        if(!checkDivision(a,arrayB) &&  a!= 1){
            answer = a;
        }
        if(!checkDivision(b,arrayA) && b!=1){
            answer = Math.max(answer,b);
        }
        
        return answer;
    }
}
```
- 최대공약수를 이용한 문제이다.
- arrayA와 arrayB의 각각의 최대 공약수 a,b를 구한다.
- a가 1이 아닌지 확인한다.
    - a가 1이면 arrayA의 최대공약수가 존재하지 않는 것이기 때문에 넘어간다.
- 그리고 arrayA 배열의 값들 중 b로 나누어 떨어지는게 있는지 확인한다.
    - 나누어 떨어지는게 없으면 answer = a;
- b가 1이 아닌지 확인한다.
    - b가 1이면 arrayB의 최대공약수가 존재하지 않는 것이기 때문에 넘어간다.
- 그리고 arrayB 배열의 값들 중 a로 나누어 떨어지는게 있는지 확인한다.
    - arrayA를 먼저 확인했기 때문에 answer와 b중 더 큰 값을 answer에 넣는다.
- answer를 return 해준다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/135807