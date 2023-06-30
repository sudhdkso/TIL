## **[1차] 셔틀버스**


#### ***problem***
카카오에서는 무료 셔틀버스를 운행하기 때문에 판교역에서 편하게 사무실로 올 수 있다. 카카오의 직원은 서로를 '크루'라고 부르는데, 아침마다 많은 크루들이 이 셔틀을 이용하여 출근한다.

이 문제에서는 편의를 위해 셔틀은 다음과 같은 규칙으로 운행한다고 가정하자.

- 셔틀은 `09:00`부터 총 `n`회 `t`분 간격으로 역에 도착하며, 하나의 셔틀에는 최대 `m`명의 승객이 탈 수 있다.
- 셔틀은 도착했을 때 도착한 순간에 대기열에 선 크루까지 포함해서 대기 순서대로 태우고 바로 출발한다. 예를 들어 `09:00`에 도착한 셔틀은 자리가 있다면 `09:00`에 줄을 선 크루도 탈 수 있다.

일찍 나와서 셔틀을 기다리는 것이 귀찮았던 콘은, 일주일간의 집요한 관찰 끝에 어떤 크루가 몇 시에 셔틀 대기열에 도착하는지 알아냈다. 콘이 셔틀을 타고 사무실로 갈 수 있는 도착 시각 중 제일 늦은 시각을 구하여라.

단, 콘은 게으르기 때문에 같은 시각에 도착한 크루 중 대기열에서 제일 뒤에 선다. 또한, 모든 크루는 잠을 자야 하므로 `23:59`에 집에 돌아간다. 따라서 어떤 크루도 다음날 셔틀을 타는 일은 없다.

#### **입력형식**
셔틀 운행 횟수 `n`, 셔틀 운행 간격 `t`, 한 셔틀에 탈 수 있는 최대 크루 수 `m`, 크루가 대기열에 도착하는 시각을 모은 배열 `timetable`이 입력으로 주어진다.

- 0 ＜ `n` ≦ 10
- 0 ＜ `t` ≦ 60
- 0 ＜ `m` ≦ 45
- `timetable`은 최소 길이 1이고 최대 길이 2000인 배열로, 하루 동안 크루가 대기열에 도착하는 시각이 `HH:MM` 형식으로 이루어져 있다.
- 크루의 도착 시각 `HH:MM`은 `00:01`에서 `23:59` 사이이다.

#### **출력형식**
콘이 무사히 셔틀을 타고 사무실로 갈 수 있는 제일 늦은 도착 시각을 출력한다. 도착 시각은 `HH:MM` 형식이며, `00:00`에서 `23:59` 사이의 값이 될 수 있다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    public String solution(int n, int t, int m, String[] timetable) {
        String answer = "";
        Arrays.sort(timetable, (o1,o2) -> {
            int t1 = convertTime(o1);
            int t2 = convertTime(o2);
            return t1-t2;
        });
        
        int arrtime = convertTime("09:00");
        int lasttime = arrtime + (n-1)*t;
        int count = 0;
        List<Integer> list = new ArrayList<>();
        int index = 0;
        while(n-- > 0){
            for(int i=index;i<timetable.length;i++){
                int num = convertTime(timetable[i]);
                if(arrtime >= num && count < m){
                    if(lasttime == arrtime){
                        list.add(num);
                    }
                    count++;
                }
                else if(arrtime < num || count >= m){
                    index = i;
                    break;
                }
            }
            count = 0;
            arrtime += t;
        }
        Collections.sort(list);
        answer = list.size() >= m ? timeNumToString(list.get(m-1)-1) : timeNumToString(lasttime);
   
        return answer;
    }
    
    private static int convertTime(String time){
        String[] s = time.split(":");
        return Integer.parseInt(s[0])*60 + Integer.parseInt(s[1]);
    }
    
    private static String timeNumToString(int num){
        int hh = num/60;
        int mm = num%60;
        String time = hh/10 == 0 ? "0" + String.valueOf(hh) : String.valueOf(hh);
        time = mm/10 == 0 ? time +":0"+String.valueOf(mm) : time+":"+String.valueOf(mm);    
        return time;
    }
}
```
- convertTime은 String으로 들어오는 `HH:MM`을 int타입으로 변경하는 함수이다.
- timeNumToString은 int타입의 시간을 `HH:MM`으로 변경하는 함수이다.
- 먼저 timetable을 오름차순으로 정렬한다.
- 오름차순으로 정렬된 timetable과 현재 도착한 셔틀 버스의 시간을 비교하며 탈 수 있는 현재 도착한 사람이 셔틀 버스를 탈 수 있는 지 확인한다.
- 셔틀 버스를 탈 수 있으면 셔틀 버스를 탄 사람의 수를 증가시킨다.
- 그리고 셔틀 버스를 탈 수 없으면 반복문을 멈추고 다음 셔틀버스 시간으로 변경한다.

- 미리 구해놓은 막차시간과 지금 온 셔틀버스의 시간이 같으면 막차시간에 탈 수 있는 사람들의 시간을 list에 넣는다.
- 이 list를 오름차순으로 정렬한다.
- 이때 list의 size가 셔틀에 탈 수 있는 크루의 수(m)보다 크면 list의 m-1번째 사람의 시간에 1분을 뺀 값이 정답이다.
- 그리고 list의 size가 m보다 작으면 막차시간에 도착하면 되므로 answer는 막차 시간이다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/17679