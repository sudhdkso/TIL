## **귤 고르기**


### ***problem***
경화는 과수원에서 귤을 수확했습니다. 경화는 수확한 귤 중 'k'개를 골라 상자 하나에 담아 판매하려고 합니다. 그런데 수확한 귤의 크기가 일정하지 않아 보기에 좋지 않다고 생각한 경화는 귤을 크기별로 분류했을 때 서로 다른 종류의 수를 최소화하고 싶습니다.

예를 들어, 경화가 수확한 귤 8개의 크기가 [1, 3, 2, 5, 4, 5, 2, 3] 이라고 합시다. 경화가 귤 6개를 판매하고 싶다면, 크기가 1, 4인 귤을 제외한 여섯 개의 귤을 상자에 담으면, 귤의 크기의 종류가 2, 3, 5로 총 3가지가 되며 이때가 서로 다른 종류가 최소일 때입니다.

경화가 한 상자에 담으려는 귤의 개수 k와 귤의 크기를 담은 배열 `tangerine`이 매개변수로 주어집니다. 경화가 귤 `k`개를 고를 때 크기가 서로 다른 종류의 수의 최솟값을 return 하도록 solution 함수를 작성해주세요.

#### **제한사항**
- 1 ≤ `k` ≤ `tangerine`의 길이 ≤ 100,000
- 1 ≤ `tangerine`의 원소 ≤ 10,000,000


### ***Solution***
``` java
import java.util.*;

class Solution {
     public static int solution(int k, int[] tangerine) {
        int answer = 0;
        int num = tangerine.length;
        int min = 999;
        Arrays.sort(tangerine);
        int length = tangerine[num-1];
        int[] chart = new int[length+1];
        for(int i=0;i<num;i++){
            chart[tangerine[i]]++;
        }
        Arrays.sort(chart);

        for(int i=length;i>0;i--){
            if(k <= 0){
                break;
            }
            k-=chart[i];
            answer++;

        }

        return answer;
    }
}
```
- chart는 귤을 크기 별로 저장하는 int형 배열에 저장을 한다.
    - chart배열의 인덱스는 귤의 크기이다.
    - chart[i]의 값은 같은 크기의 귤의 갯수이다.
- chart를 오름차순으로 정렬해준다.
- 그리고 chart의 마지막 부터 반복문을 돌며 많은 갯수를 가지고 있는 크기부터 k에서 빼준다. 

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/87946