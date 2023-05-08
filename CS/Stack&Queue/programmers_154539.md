## **뒤에 있는 큰 수 찾기**


### ***problem***
정수로 이루어진 배열 `numbers`가 있습니다. 배열 의 각 원소들에 대해 자신보다 뒤에 있는 숫자 중에서 자신보다 크면서 가장 가까이 있는 수를 뒷 큰수라고 합니다.
정수 배열 `numbers`가 매개변수로 주어질 때, 모든 원소에 대한 뒷 큰수들을 차례로 담은 배열을 return 하도록 solution 함수를 완성해주세요. 단, 뒷 큰수가 존재하지 않는 원소는 -1을 담습니다.




#### **제한사항**
- 4 ≤ `numbers`의 길이 ≤ 1,000,000
    - 1 ≤ `numbers[i]` ≤ 1,000,000

### ***Solution***
``` java
import java.util.Stack;
class Solution {
    private class Pair{
        int index,value;
        public Pair(int index, int value){
            this.index = index;
            this.value = value;
        }
    }
    
    public int[] solution(int[] numbers) {
        int len = numbers.length;
        int[] answer = new int[len];
        Stack<Integer> s = new Stack<>();
        
        for(int i=len-1;i>=0;i--){
            while(!s.isEmpty()){
                if(s.peek() <= numbers[i]){
                    s.pop();
                }
                else{
                    answer[i] = s.peek();
                    s.push(numbers[i]);
                    break;
                }
            }
            
            if(s.isEmpty()){
                answer[i] = -1;
                s.push(numbers[i]);
                continue;
            }
            
        }
        
        
        return answer;
    }
}
```
- 반복문을 뒤에서부터 시작하여 0까지 돈다.
    - stack이 비어있는지 확인한다.
    - stack이 비어있지 않으면 stack의 맨 앞이 지금 numbers[i]보다 작거나 같으면 s에 있는 것을 빼낸다.
    - stack의 맨앞에 지금 numbers[i]보다 크면 answer[i]에 stack의 맨 앞을 저장하고, stack에 numbers[i]를 넣어준다.
    
    - stack에 대한 while문에서 빠져나온 후 stack이 비어있다면 numbers[i]보다 큰 값이 없기 때문에 answer[i]에 -1를 넣고 stack에 numbers[i]를 추가한다.  

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/154539