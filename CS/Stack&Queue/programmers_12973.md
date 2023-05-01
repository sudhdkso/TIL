## **짝지어 제거하기**


### ***problem***
짝지어 제거하기는, 알파벳 소문자로 이루어진 문자열을 가지고 시작합니다. 먼저 문자열에서 같은 알파벳이 2개 붙어 있는 짝을 찾습니다. 그다음, 그 둘을 제거한 뒤, 앞뒤로 문자열을 이어 붙입니다. 이 과정을 반복해서 문자열을 모두 제거한다면 짝지어 제거하기가 종료됩니다. 문자열 S가 주어졌을 때, 짝지어 제거하기를 성공적으로 수행할 수 있는지 반환하는 함수를 완성해 주세요. 성공적으로 수행할 수 있으면 1을, 아닐 경우 0을 리턴해주면 됩니다.

예를 들어, 문자열 S = `baabaa` 라면

b aa baa → bb aa → aa →

의 순서로 문자열을 모두 제거할 수 있으므로 1을 반환합니다.


#### **제한사항**
- 문자열의 길이 : 1,000,000이하의 자연수
- 문자열은 모두 소문자로 이루어져 있습니다.



### ***Solution***
``` java
import java.util.Stack;

class Solution {

    public int solution(String s) {
        int len = s.length();
        Stack<Character> st = new Stack<>();
        
        for(int i=0;i<len;i++){
            if(st.isEmpty()){
                st.add(s.charAt(i));
            }
            else{
                if(st.peek() == s.charAt(i)){
                    st.pop();
                }
                else{
                    st.add(s.charAt(i));
                }
            }
        }
        
        if(st.isEmpty()) return 1;
        else return 0;
    }
}
```
- Stack st에 s문자열의 값을 하나하나 넣으면서 st의 가장위에 있는 값이 st에 넣으려는 값과 같으면 st.pop()을 하고 아니면 st에 추가하는 방식으로 반복문을 수행한다.
- 가장 마지막까지 반복했을 때 st가 비어있으면 모두 짝지어진것이고 비어있지 않으면 모두 짝지어지지 않은것이다. 

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/12973