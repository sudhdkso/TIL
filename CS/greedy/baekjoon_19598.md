### **최소 회의실 개수**


#### ***problem***
서준이는 아빠로부터 N개의 회의를 모두 진행할 수 있는 최소 회의실 개수를 구하라는 미션을 받았다. 각 회의는 시작 시간과 끝나는 시간이 주어지고 한 회의실에서 동시에 두 개 이상의 회의가 진행될 수 없다. 단, 회의는 한번 시작되면 중간에 중단될 수 없으며 한 회의가 끝나는 것과 동시에 다음 회의가 시작될 수 있다. 회의의 시작 시간은 끝나는 시간보다 항상 작다. N이 너무 커서 괴로워 하는 우리 서준이를 도와주자.

#### Input
첫째 줄에 배열의 크기 N(1 ≤ N ≤ 100,000)이 주어진다. 둘째 줄부터 N+1 줄까지 공백을 사이에 두고 회의의 시작시간과 끝나는 시간이 주어진다. 시작 시간과 끝나는 시간은 231−1보다 작거나 같은 자연수 또는 0이다.

#### Output
첫째 줄에 최소 회의실 개수를 출력한다.

### ***Solution***
``` java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.*;

public class Main {
    private static class Time{
        int start,end;
        public Time(int start, int end){
            this.start = start;
            this.end = end;
        }
    }

    public static void main( String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        PriorityQueue<Time> pq = new PriorityQueue<Time>((o1,o2) -> {
            if(o1.start == o2.start){
                return o1.end-o2.end;
            }
            return o1.start-o2.start;
        });

        int n = Integer.parseInt(br.readLine());
        int[] arr = new int[n];

        for(int i=0;i<n;i++){
            String[] SE = br.readLine().split(" ");
            pq.add(new Time(Integer.parseInt(SE[0]),Integer.parseInt(SE[1])));
        }

        int count = 0;
        while(!pq.isEmpty()){
            Time t = pq.poll();

            for(int i=0;i<n;i++){
                if(arr[i] == 0){
                    count++;
                    arr[i] = t.end;
                    break;
                }
                if(arr[i] <= t.start){
                    arr[i] = t.end;
                    break;
                }
            }
        }
        bw.write(count+"\n");
        bw.flush();
    }

}
```
- Time은 회의시간에 대한 class이다.
- arr은 회의실을 의미하는 int형 일차원 배열이다. 	
	
    - length는 회의실이 최대로 필요한 최악의 경우이다.
    
- priorityQueue는 Time을 받으며 우선순위는 회의 시간의 시작시간을 기준으로 오름차순으로 정렬한다.
- pq가 비어있을 때 까지 아래행동을 반복한다.
   - pq의 맨앞에 있는 time t를 꺼낸다.
 
    - arr를 돌며 arr[i]가 0이면 배정된 회의가 없으므로 arr[i]에 t.end를 저장하고 count를 1증가시키고 반복문을 탈출한다.
    
    - arr[i]가 0 이아니면 arr[i]의 값과 t.start의 값을 비교한다. 다음회의 시작 시간이 arr[i]에 들어있는 회의 종료시간보다 크거나 같으면 arr[i]에 t.end를 저장하고 반복문을 탈출한다.

- 이렇게 pq가 비어 있을때까지 반복문을 돌면 회의시간에 맞게 최소한의 회의실 갯수가 count에 저장된다.
### 출처
https://www.acmicpc.net/problem/19598
