## **디스크 컨트롤러**

#### ***problem***
하드디스크는 한 번에 하나의 작업만 수행할 수 있습니다. 디스크 컨트롤러를 구현하는 방법은 여러 가지가 있습니다. 가장 일반적인 방법은 요청이 들어온 순서대로 처리하는 것입니다.

예를들어
```
- 0ms 시점에 3ms가 소요되는 A작업 요청
- 1ms 시점에 9ms가 소요되는 B작업 요청
- 2ms 시점에 6ms가 소요되는 C작업 요청
```
와 같은 요청이 들어왔습니다. 이를 그림으로 표현하면 아래와 같습니다.

<p align="center">
<img width="500px" src="https://grepp-programmers.s3.amazonaws.com/files/production/b68eb5cec6/38dc6a53-2d21-4c72-90ac-f059729c51d5.png">
</p>

한 번에 하나의 요청만을 수행할 수 있기 때문에 각각의 작업을 요청받은 순서대로 처리하면 다음과 같이 처리 됩니다.

<p align="center">
<img width="500px" src="https://grepp-programmers.s3.amazonaws.com/files/production/9eb7c5a6f1/a6cff04d-86bb-4b5b-98bf-6359158940ac.png">
</p>

```
- A: 3ms 시점에 작업 완료 (요청에서 종료까지 : 3ms)
- B: 1ms부터 대기하다가, 3ms 시점에 작업을 시작해서 12ms 시점에 작업 완료(요청에서 종료까지 : 11ms)
- C: 2ms부터 대기하다가, 12ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 16ms)
```

이 때 각 작업의 요청부터 종료까지 걸린 시간의 평균은 10ms(= (3 + 11 + 16) / 3)가 됩니다.

하지만 A → C → B 순서대로 처리하면

<p align="center">
<img width="500px" src="https://grepp-programmers.s3.amazonaws.com/files/production/5e677b4646/90b91fde-cac4-42c1-98b8-8f8431c52dcf.png">
</p>

```
- A: 3ms 시점에 작업 완료(요청에서 종료까지 : 3ms)
- C: 2ms부터 대기하다가, 3ms 시점에 작업을 시작해서 9ms 시점에 작업 완료(요청에서 종료까지 : 7ms)
- B: 1ms부터 대기하다가, 9ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 17ms)
```
이렇게 A → C → B의 순서로 처리하면 각 작업의 요청부터 종료까지 걸린 시간의 평균은 9ms(= (3 + 7 + 17) / 3)가 됩니다.

각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때, 작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로 처리하면 평균이 얼마가 되는지 return 하도록 solution 함수를 작성해주세요. (단, 소수점 이하의 수는 버립니다)

#### **제한사항**
- jobs의 길이는 1 이상 500 이하입니다.
- jobs의 각 행은 하나의 작업에 대한 [작업이 요청되는 시점, 작업의 소요시간] 입니다.
- 각 작업에 대해 작업이 요청되는 시간은 0 이상 1,000 이하입니다.
- 각 작업에 대해 작업의 소요시간은 1 이상 1,000 이하입니다.
- 하드디스크가 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리합니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    static Queue<Job> ready = new PriorityQueue<>((o1,o2) -> {
        return o1.time-o2.time;
    });
    
    public int solution(int[][] jobs) {
        int answer = 0;
        int length = jobs.length;
        int index = 0;

        int ctime = 0, total = 0;
        Arrays.sort(jobs,(o1,o2) ->{
            if(o1[0] == o2[0]){
                return o1[1]-o2[1];
            }
            return o1[0]-o2[0];
        });
            
        while(true){
            if(index >= length && ready.isEmpty()){
                break;
            }
            for(int i = index;i<length;i++){
                if(jobs[i][0] <= ctime){
                    ready.offer(new Job(jobs[i][0],jobs[i][1]));
                    index++;
                }
            }
            
            if(ready.isEmpty()){
                ctime +=1;
                continue;
            }

            if(!ready.isEmpty()){
                Job job = ready.poll();
                ctime += job.time;
                total += ctime - job.start;
            }
            
        }
        
        return total/length;
    }
    
    private static class Job{
        int start,time;
        public Job(int start, int time){
            this.start = start;
            this.time = time;
        }
        
        @Override
        public String toString(){
            return "["+this.start+" "+this.time+"]";
        }
    }
}
```
- `ready`는 요청 시간이 짧은 시간을 우선순위로 하는 우선순위 큐이다.
- 먼저 `jobs`를 요청 시간을 기준으로 정렬한다.
- ctime은 현재 시간, total은 일들의 요청시간부터 종료시간까지를 모두 더한 값이다.
1. 먼저 ctime을 기준으로 현재 처리할 수 있는 일들을 ready에 넣는다.
2. 그리고 ready가 지금 처리할 수 있는 일이 있으면 ready의 맨앞 일을 먼저 처리한다.
    - 이때 ctime은 ctime+job.time이 된다.
    - 그리고 total은 total + ctime -job.start이다.
3. 처리할 수 있는 일이 없으면 ctime을 1 증가시킨다.
4. 그리고 다시 처음으로 돌아가 갱신된 현재시간에 처리할 수 있는 일들을 다시 ready에 넣는다.
    - 2번에서 ready에 있는 일들을 모두 처리하지 않고 하나만 처리한 이유는 일을 처리함으로써 현재시간이 증가하여, 남아있는 일들중에 현재시간에 처리할 수 있는 일들이 존재할 수 있기 때문이다.
    - 그 일들중에 ready에 있는 값들 보다 더 빨리 종료되는 일이 존재할 수 있기 때문에 하나의 일을 처리하고 다시 1번부터 반복한다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42627?language=java#