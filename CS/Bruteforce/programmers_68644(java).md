## **혼자서 하는 틱택토**


### ***problem***
정수 배열 numbers가 주어집니다. numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.

### **제한사항**
- numbers의 길이는 2 이상 100 이하입니다.
    - numbers의 모든 수는 0 이상 100 이하입니다.

### ***Solution***
``` java
import java.util.List;
import java.util.ArrayList;
class Solution {
    public int[] solution(int[] numbers) {
        int n = numbers.length;
        List<Integer> list = new ArrayList<>();
        for(int i=0;i<n;i++){
            int a = numbers[i];
            for(int j=i+1;j<n;j++){
                int num = a + numbers[j];
                if(!list.contains(num)){
                    list.add(num);
                }
            }
        }
        list.sort((o1,o2) -> o1-o2);
        
        return list.stream().mapToInt(Integer::intValue).toArray();
    }
}
```
- 완전탐색을 통해 모든 경우를 다 확인하는 방법을 선택하였다.
- numbers[i]와 numbers[j]~numbers[n-1]을 더해서 list에 값이 있으면 넣지 않고 list에 값이 없으면 넣는 방식으로 numbers에서 두개의 값을 뽑아 더해서 나올 수 있는 모든 값을 구한다.
    - 이때 j는 `(i+1 <= j < n)`이다.
- 완전탐색을 통해 구한 list를 오름차순으로 정렬하고, 반환타입에 맞게 List를 Array로 변환하여 return한다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/68644?language=java