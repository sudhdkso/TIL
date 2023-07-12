## **$n^2$배열 자르기**


#### ***problem***
정수 `n`, `left`, `right`가 주어집니다. 다음 과정을 거쳐서 1차원 배열을 만들고자 합니다.

1. `n`행 `n`열 크기의 비어있는 2차원 배열을 만듭니다.
2. `i = 1, 2, 3, ..., n`에 대해서, 다음 과정을 반복합니다.
    - 1행 1열부터 `i`행 `i`열까지의 영역 내의 모든 빈 칸을 숫자 `i`로 채웁니다.
3. 1행, 2행, ..., `n`행을 잘라내어 모두 이어붙인 새로운 1차원 배열을 만듭니다.
4. 새로운 1차원 배열을 a`rr`이라 할 때, `arr[left]`, `arr[left+1]`, ..., `arr[right]`만 남기고 나머지는 지웁니다.

정수 `n`, `left`, `right`가 매개변수로 주어집니다. 주어진 과정대로 만들어진 1차원 배열을 return 하도록 solution 함수를 완성해주세요.



#### **제한사항**
- 1 ≤ `n` ≤ $10^7$
- 0 ≤ `left` ≤ `right` < $n^2$
- `right` - `left` < $10^5$
### ***Solution***
``` java
class Solution {
    public int[] solution(int n, long left, long right) {
        int length = (int)right - (int)left + 1;
        int[] answer = new int[length];
        for(int i=0;i<length;i++){
            answer[i] = Math.max((int)(left/n),(int)(left%n))+1;
            left += 1;
        }
        return answer;
    }
}
```


### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/87390