## **호텔 대실**


#### ***problem***
호텔을 운영 중인 코니는 최소한의 객실만을 사용하여 예약 손님들을 받으려고 합니다. 한 번 사용한 객실은 퇴실 시간을 기준으로 10분간 청소를 하고 다음 손님들이 사용할 수 있습니다.
예약 시각이 문자열 형태로 담긴 2차원 배열 `book_time`이 매개변수로 주어질 때, 코니에게 필요한 최소 객실의 수를 return 하는 solution 함수를 완성해주세요.

#### **제한사항**
- 1 ≤ `book_time`의 길이 ≤ 1,000
    - `book_time[i]`는 ["HH:MM", "HH:MM"]의 형태로 이루어진 배열입니다
        - [대실 시작 시각, 대실 종료 시각] 형태입니다.
    - 시각은 HH:MM 형태로 24시간 표기법을 따르며, "00:00" 부터 "23:59" 까지로 주어집니다.
        - 예약 시각이 자정을 넘어가는 경우는 없습니다.
        - 시작 시각은 항상 종료 시각보다 빠릅니다.


### ***Solution***
``` java
import java.util.*;

class Solution {
    private static class Pair {
        int start,end;

        public Pair(String start, String end){
            this.start = Integer.parseInt(start.split(":")[0])*60 +Integer.parseInt(start.split(":")[1]);
            this.end = Integer.parseInt(end.split(":")[0])*60 +Integer.parseInt(end.split(":")[1])+10;

        }
    }

    public static int solution(String[][] book_time) {
        int answer = 0;

        PriorityQueue<Pair> pq = new PriorityQueue<Pair>((pq1 ,pq2) -> {
            if(pq1.start == pq2.start){
                return pq1.end - pq2.end;
            }
            return pq1.start - pq2.start;
        });

        int length = book_time.length;
        int[] room_list = new int[length];

        for(int i=0;i<length;i++){
            pq.add(new Pair(book_time[i][0], book_time[i][1]));
        }
        int room_num = 0;
        while(!pq.isEmpty()){
            Pair p = pq.poll();

            for(int i=0;i<length;i++){
                if(room_list[i] == 0){
                    room_num++;
                    room_list[i] = p.end;
                    break;
                }
                
                if(room_list[i] <= p.start ){
                    room_list[i] = p.end;
                    break;
                }
            }
        }
        answer = room_num;
        return answer;
    }

}
```
- Pair클래스는 호텔 대실시간을 int형으로 변경하여 가지고 있는 클래스이다.
- pq는 PriorityQueue로 start시간에 대해서 오름차순하고 start시간이 같다면 end시간에 대해서 오름차순하여 queue를 만들어주는 우선순위 큐이다.
- 호텔 대실 시간을 모두 pq에 저장한다.
- room_list는 객실에 대한 정보를 가지고 있는 int형 일차원 배열이다.
- pq에 대해서 pq가 비어있을 때 까지 반복문을 돌린다.
    - room_list를 차례대로 돌며 start 시간이 room_list에 저장된 값 보다 큰지 확인한다.
    - 이때 room_list가 0이면 새 방으로 room_list[i]에 end값을 저장하고 room_num을 1 증가시킨다.
    - room_list가 start보다 작거나 같으면 그 방을 사용할 수 있으므로 그냥 room_list[i]에 end를 저장해준다. 
        - 새방을 사용하는 것은 아니므로 room_num은 증가시키지 않는다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/155651