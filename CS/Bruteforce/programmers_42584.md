## **주식가격**


### ***problem***
초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

### **제한사항**
- - prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.
- prices의 길이는 2 이상 100,000 이하입니다.

### ***Solution***
``` java
class Solution {
    public int[] solution(int[] prices) {
        int len = prices.length;
        int[] answer = new int[len];
        
        for(int i=0;i<len;i++){
            for(int j=i+1;j<len;j++){
                answer[i]++;
                if(prices[i] > prices[j]){
                    break;
                }
            }
        }
        return answer;
    }
}
```
### **문제 풀이** 
- prices[i]가 몇초까지 유지가 되는지 j를 통해서 체크한다.
- 이때 j를 통해 확인하며 answer[i]를 증가시킨다.
- prices[i] > prices[j]가 되면 가격이 떨어져서 prices[i]의 가격을 유지하지 못하므로 반복문을 탈출한다.
- 그리고 다음 가격을 확인하기 위해 위의 과정을 반복한다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42584