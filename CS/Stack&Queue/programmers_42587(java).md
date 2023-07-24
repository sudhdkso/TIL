## **프로세스**


### ***problem***
운영체제의 역할 중 하나는 컴퓨터 시스템의 자원을 효율적으로 관리하는 것입니다. 이 문제에서는 운영체제가 다음 규칙에 따라 프로세스를 관리할 경우 특정 프로세스가 몇 번째로 실행되는지 알아내면 됩니다.
``` markdown
1. 실행 대기 큐(Queue)에서 대기중인 프로세스 하나를 꺼냅니다.
2. 큐에 대기중인 프로세스 중 우선순위가 더 높은 프로세스가 있다면 방금 꺼낸 프로세스를 다시 큐에 넣습니다.
3. 만약 그런 프로세스가 없다면 방금 꺼낸 프로세스를 실행합니다.
  3.1 한 번 실행한 프로세스는 다시 큐에 넣지 않고 그대로 종료됩니다.
```
예를 들어 프로세스 4개 [A, B, C, D]가 순서대로 실행 대기 큐에 들어있고, 우선순위가 [2, 1, 3, 2]라면 [C, D, A, B] 순으로 실행하게 됩니다.

현재 실행 대기 큐(Queue)에 있는 프로세스의 중요도가 순서대로 담긴 배열 `priorities`와, 몇 번째로 실행되는지 알고싶은 프로세스의 위치를 알려주는 `location`이 매개변수로 주어질 때, 해당 프로세스가 몇 번째로 실행되는지 return 하도록 solution 함수를 작성해주세요.


#### **제한사항**
- `priorities`의 길이는 1 이상 100 이하입니다.
    - `priorities`의 원소는 1 이상 9 이하의 정수입니다.
    - `priorities`의 원소는 우선순위를 나타내며 숫자가 클 수록 우선순위가 높습니다.
- `location`은 0 이상 (대기 큐에 있는 프로세스 수 - 1) 이하의 값을 가집니다.
    - `priorities`의 가장 앞에 있으면 0, 두 번째에 있으면 1 … 과 같이 표현합니다.

### ***Solution***
``` java
import java.util.*;
import java.util.stream.Collectors;
class Solution {
    private class Printer{
        int index,priority;
        public Printer(int index, int priority){
            this.index = index;
            this.priority = priority;
        }
    }
    
    public int solution(int[] priorities, int location) {
        int answer = 0;
        Queue<Printer>q = new LinkedList<>();
        List<Integer> list = Arrays.stream(priorities).boxed().collect(Collectors.toList());
        
        int len = priorities.length;
        list.sort(Comparator.reverseOrder());
        for(int i=0;i<len;i++){
            q.offer(new Printer(i,priorities[i]));
        }
        
        while(!q.isEmpty()){
            Printer p = q.poll();
            if(p.priority < list.get(0)){
                q.offer(p);
            }
            else{
                answer++;
                list.remove(0);
                if(location == p.index){
                    break;
                }
            }
        }
        
        return answer;
    }
}
```
#### 클래스 없이 단순 큐만 사용한 풀이
> 클래스를 사용하지 않을 때는 location의 위치를 계속 옮겨야한다는 불편함이 존재했다. 
<br/>하지만 list를 통해서 큐에 한번에 원소들을 순서대로 넣을 수 있었기 때문에, 각각의 방법은 장단점이 존재한다고 생각한다. 

``` java
import java.util.*;
import java.util.stream.*;
class Solution {
    public int solution(int[] priorities, int location) {
        int answer = 0;
        List<Integer> list = Arrays.stream(priorities).boxed().collect(Collectors.toList());
        
        Queue<Integer> q = new LinkedList<>(list);
        
        list.sort(Collections.reverseOrder());
        
        int len = priorities.length;

        while(!q.isEmpty()){
            
            if(q.peek() == list.get(0)){
                answer++;
                q.poll();
                list.remove(0);
                
                if(location == 0){
                    break;
                }
            }
            else{
                q.offer(q.poll());
            }
            location = location-1 < 0 ? q.size()-1 : location-1;
        }
        
        
        return answer;
    }
}
```
- Printer라는 class는 각 프린터의 index와 priority를 가지고 있다.
- Priorities를 List에 넣어서 내림차순으로 정렬한다.
    - 가장 큰 Priority가 맨 앞에 오도록한다.
- Queue에 Printer를 하나씩 넣어서 Queue가 비어있을 때까지 반복문을 돌린다.
    - Queue의 맨 앞에 있는 프린터의 Priority가 list.get(0)보다 작으면 Queue의 맨뒤에 다시 넣어준다.
    - Queue의 맨 앞에 있는 프린터의 Priority가 list.get(0)과 같으면  answer를 증가시키고 list의 맨앞에 항목을 삭제한다.
        - 이때 Queue의 맨 앞에 있는 프린터의 index가 location과 같으면 break한다.
            - 원하는 순번이기 때문
    
### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42587