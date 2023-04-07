## **연속된 부분 수열의 합**


#### ***problem***
비내림차순으로 정렬된 수열이 주어질 때, 다음 조건을 만족하는 부분 수열을 찾으려고 합니다.

- 기존 수열에서 임의의 두 인덱스의 원소와 그 사이의 원소를 모두 포함하는 부분 수열이어야 합니다.
- 부분 수열의 합은 `k`입니다.
- 합이 `k`인 부분 수열이 여러 개인 경우 길이가 짧은 수열을 찾습니다.
- 길이가 짧은 수열이 여러 개인 경우 앞쪽(시작 인덱스가 작은)에 나오는 수열을 찾습니다.
수열을 나타내는 정수 배열 `sequence`와 부분 수열의 합을 나타내는 정수 `k`가 매개변수로 주어질 때, 위 조건을 만족하는 부분 수열의 시작 인덱스와 마지막 인덱스를 배열에 담아 return 하는 solution 함수를 완성해주세요. 이때 수열의 인덱스는 0부터 시작합니다.



#### **제한사항**
- 5 ≤ `sequence`의 길이 ≤ 1,000,000
    - 1 ≤ `sequence`의 원소 ≤ 1,000
    - `sequence`는 비내림차순으로 정렬되어 있습니다.
- 5 ≤ `k` ≤ 1,000,000,000
`k`는 항상 `sequence`의 부분 수열로 만들 수 있는 값입니다.


### ***Solution***
``` java
class Solution {
    public static int[] solution(int[] sequence, int k) {
        int[] answer = new int[2];
        int[] cumSum = calPrefixSum(sequence);
        
        int length = cumSum.length;
        
        answer[0] = 0; answer[1] = 999;
        int left = 0, right = 0;
        int len = Integer.MAX_VALUE;
        
        while(left <= right && left < length && right < length){
            int result = cumSum[right] - cumSum[left];
            
            if(result == k){
                int nowLen = (right-1) - left;
                if(len > nowLen){
                    len = nowLen;
                    answer[0] = left;
                    answer[1] = right-1;
                }
                right++;
            }
            else if(result < k){
                right++;
            }
            else{
                left++;
            }

        }
        
        return answer;
    }

    public static int[] calPrefixSum(int[] sequence){
        int length = sequence.length;
        int[] result = new int[length+1];
        result[0] = 0;
        for(int i=1;i<length+1;i++){
            result[i] = sequence[i-1] + result[i-1];
        }
        return result;
    }
}
```
- 누적 합과 두포인터를 이용한 문제이다.
- calPrefixSum함수는 sequence를 매개변수로 가지며 sequence 배열의 누적합 배열을 만들어서 return하는 함수이다.
- calPrefixSum함수를 통해 만들어진 누적합 배열을 cumSum배열에 저장한다.
- 투포인터로 활용한 left와 right를 0으로 초기화한다.

- result에 (left, rigth) 구간 까지의 합인 cumSum[right] - cumSum[left]를 저장한다.
    - result가 k와 같으면 현재 구간의 길이를 nowLen에 저장하고 Len과 비교를 한다.
        - len보다 작으면 answer와 Len을 (left,right), nowLen으로 초기화 한다.
        - rigth를 1증가 시킨다.
    - result가 k보다 작으면 right를 1 증가시킨다.
    - 그 외의 경우 left를 1증가 시킨다.

    - 포인터 left가 right보다 작으면서 두 포인터가 cumSum의 인덱스를 넘어가지 않으면 계속 반복한다.


### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/178870#