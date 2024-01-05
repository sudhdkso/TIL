## **상담원 인원**


#### ***problem***
<div>
      <h6>문제 설명</h6>
      <div><p>현대모비스는 우수한 SW 인재 채용을 위해 상시로 채용 설명회를 진행하고 있습니다. 채용 설명회에서는 채용과 관련된 상담을 원하는 참가자에게 멘토와 1:1로 상담할 수 있는 기회를 제공합니다. 채용 설명회에는 멘토 <code>n</code>명이 있으며, 1~<code>k</code>번으로 분류되는 상담 유형이 있습니다. 각 멘토는 <code>k</code>개의 상담 유형 중 하나만 담당할 수 있습니다. 멘토는 자신이 담당하는 유형의 상담만 가능하며, 다른 유형의 상담은 불가능합니다. 멘토는 동시에 참가자 한 명과만 상담 가능하며, 상담 시간은 정확히 참가자가 요청한 시간만큼 걸립니다. </p>

<p>참가자가 상담 요청을 하면 아래와 같은 규칙대로 상담을 진행합니다.</p>

<ul>
<li>상담을 원하는 참가자가 상담 요청을 했을 때, 참가자의 상담 유형을 담당하는 멘토 중 상담 중이 아닌 멘토와 상담을 시작합니다.</li>
<li>만약 참가자의 상담 유형을 담당하는 멘토가 모두 상담 중이라면, 자신의 차례가 올 때까지 기다립니다. <strong>참가자가 기다린 시간은 참가자가 상담 요청했을 때부터 멘토와 상담을 시작할 때까지의 시간입니다.</strong> </li>
<li>모든 멘토는 상담이 끝났을 때 자신의 상담 유형의 상담을 받기 위해 기다리고 있는 참가자가 있으면 즉시 상담을 시작합니다. 이때, <strong>먼저 상담 요청한 참가자가 우선됩니다.</strong></li>
</ul>

<p>참가자의 상담 요청 정보가 주어질 때, 참가자가 상담을 요청했을 때부터 상담을 시작하기까지 기다린 시간의 합이 최소가 되도록 각 상담 유형별로 멘토 인원을 정하려 합니다. <strong>단, 각 유형별로 멘토 인원이 적어도 한 명 이상이어야 합니다.</strong> </p>

<p>예를 들어, 5명의 멘토가 있고 1~3번의 3가지 상담 유형이 있을 때 아래와 같은 참가자의 상담 요청이 있습니다.</p>

<p><strong>참가자의 상담 요청</strong></p>
<table class="table">
        <thead><tr>
<th>참가자 번호</th>
<th>시각</th>
<th>상담 시간</th>
<th>상담 유형</th>
</tr>
</thead>
        <tbody><tr>
<td>1번 참가자</td>
<td>10분</td>
<td>60분</td>
<td>1번 유형</td>
</tr>
<tr>
<td>2번 참가자</td>
<td>15분</td>
<td>100분</td>
<td>3번 유형</td>
</tr>
<tr>
<td>3번 참가자</td>
<td>20분</td>
<td>30분</td>
<td>1번 유형</td>
</tr>
<tr>
<td>4번 참가자</td>
<td>30분</td>
<td>50분</td>
<td>3번 유형</td>
</tr>
<tr>
<td>5번 참가자</td>
<td>50분</td>
<td>40분</td>
<td>1번 유형</td>
</tr>
<tr>
<td>6번 참가자</td>
<td>60분</td>
<td>30분</td>
<td>2번 유형</td>
</tr>
<tr>
<td>7번 참가자</td>
<td>65분</td>
<td>30분</td>
<td>1번 유형</td>
</tr>
<tr>
<td>8번 참가자</td>
<td>70분</td>
<td>100분</td>
<td>2번 유형</td>
</tr>
</tbody>
      </table>
<p>이때, 멘토 인원을 아래와 같이 정하면, 참가자가 기다린 시간의 합이 25로 최소가 됩니다.</p>
<table class="table">
        <thead><tr>
<th>1번 유형</th>
<th>2번 유형</th>
<th>3번 유형</th>
</tr>
</thead>
        <tbody><tr>
<td>2명</td>
<td>1명</td>
<td>2명</td>
</tr>
</tbody>
      </table>
<p><strong>1번 유형</strong></p>

<p>1번 유형을 담당하는 멘토가 2명 있습니다.</p>

<ul>
<li>1번 참가자가 상담 요청했을 때, 멘토#1과 10분~70분 동안 상담을 합니다.</li>
<li>3번 참가자가 상담 요청했을 때, 멘토#2와 20분~50분 동안 상담을 합니다.</li>
<li>5번 참가자가 상담 요청했을 때, 멘토#2와 50분~90분 동안 상담을 합니다.</li>
<li>7번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 1번 참가자의 상담이 끝날 때까지 5분 동안 기다리고 멘토#1과 70분~100분 동안 상담을 합니다. </li>
</ul>

<p><strong>2번 유형</strong></p>

<p>2번 유형을 담당하는 멘토가 1명 있습니다.</p>

<ul>
<li>6번 참가자가 상담 요청했을 때, 멘토와 60분~90분 동안 상담을 합니다.</li>
<li>8번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 6번 참가자의 상담이 끝날 때까지 20분 동안 기다리고 90분~190분 동안 상담을 합니다.</li>
</ul>

<p><strong>3번 유형</strong></p>

<p>3번 유형을 담당하는 멘토가 2명 있습니다.</p>

<ul>
<li>2번 참가자가 상담 요청했을 때, 멘토#1과 15분~115분 동안 상담을 합니다.</li>
<li>4번 참가자가 상담 요청했을 때, 멘토#2와 30분~80분 동안 상담을 합니다.</li>
</ul>

<p>상담 유형의 수를 나타내는 정수 <code>k</code>, 멘토의 수를 나타내는 정수 <code>n</code>과 참가자의 상담 요청을 담은 2차원 정수 배열 <code>reqs</code>가 매개변수로 주어집니다. 멘토 인원을 적절히 배정했을 때 참가자들이 상담을 받기까지 기다린 시간을 모두 합한 값의 최솟값을 return 하도록 solution 함수를 완성해 주세요.</p>

<hr>

#### **제한사항**
<ul>
<li>1 ≤ <code>k</code> ≤ 5</li>
<li><code>k</code> ≤ <code>n</code> ≤ 20</li>
<li>3 ≤ <code>reqs</code>의 길이 ≤ 300

<ul>
<li><code>reqs</code>의 원소는 [<code>a</code>, <code>b</code>, <code>c</code>] 형태의 길이가 3인 정수 배열이며, <code>c</code>번 유형의 상담을 원하는 참가자가 <code>a</code>분에 <code>b</code>분 동안의 상담을 요청했음을 의미합니다.</li>
<li>1 ≤ <code>a</code> ≤ 1,000</li>
<li>1 ≤ <code>b</code> ≤ 100</li>
<li>1 ≤ <code>c</code> ≤ <code>k</code></li>
<li><code>reqs</code>는 <code>a</code>를 기준으로 오름차순으로 정렬되어 있습니다.</li>
<li><code>reqs</code> 배열에서 <code>a</code>는 중복되지 않습니다. 즉, 참가자가 상담 요청한 시각은 모두 다릅니다.</li>
</ul></li>
</ul>

### ***Solution***
``` java
import java.util.*;

class Solution {
    private class Counsel implements Comparable<Counsel>{
        int start;
        int end;
        
        public Counsel(int start, int end){
            this.start = start;
            this.end = end;
        }
        
        public boolean isEnd(int time){
            return time >= end;
        }
        
        @Override
        public int compareTo(Counsel c){
            return Integer.compare(this.end, c.end);
        }
    }
    
    public int solution(int k, int n, int[][] reqs) {
        
        int answer = 0;
        //남아있는 상담원 인원
        int remain = n - k;
                
        Map<Integer, List<Counsel>> map = setCounsel(reqs);
        
        //유형 별 상담원 수의 따른 대기 시간
        int[][] waitTimeOfType = getTotalWaitTimeOfType(n, k , map);
        
        //상담 유형별 할당되는 상담원의 수
        // i -> i유형의 상담원 수
        int[] counselors = new int[k+1];  
        Arrays.fill(counselors, 1);
        
        while(remain-- > 0){
            int maxReduce = 0;
            int index = 0;
            for(int type=1;type<=k;type++){
                int count = counselors[type];
                int before = waitTimeOfType[type][count];
                
                int after = waitTimeOfType[type][count+1];
                
                int reduce = before - after;
                if(maxReduce <= reduce){
                    maxReduce = reduce;
                    index = type;
                }
            }
            counselors[index]++;
        }
        
        return sumTotalWaitTime(counselors, waitTimeOfType);
    }
    
    //상담 유형별 상담 시간을 분류하는 함수
    private Map<Integer, List<Counsel>> setCounsel(int[][] reqs){
        Map<Integer, List<Counsel>> map = new HashMap<>();
        
        for(int[] req : reqs){
            int start = req[0];
            int time = req[1];
            int type = req[2];
            
            map.put(type, map.getOrDefault(type, new ArrayList<Counsel>()));
            map.get(type).add(new Counsel(start, start+time)); 
        }
        
        return map;
    }
    
    private int[][] getTotalWaitTimeOfType(int n, int k, Map<Integer, List<Counsel>> map){
        int maxCounselor = n-k+1;
        int[][] waittingTime = new int[n+1][maxCounselor+1];
        //각 유형별 상담원의 수에 따른 대기 시간
        //(i,j) -> i타입의 상담원이 j명있을 때 대기 시간
        for(int type=1;type<=k;type++){
            for(int count=1;count<=maxCounselor;count++){
                if(!map.containsKey(type)){
                    continue;
                }
                waittingTime[type][count] = calcWaitTime(map.get(type), count);
            }
        }
        return waittingTime;
    }
    
    //상담원의 수에 따른 상담 대기 시간을 구하는 함수
    private int calcWaitTime(List<Counsel> counseles, int count){
        int wait = 0;
        PriorityQueue<Counsel> pq = new PriorityQueue<>();

        for(Counsel counsel : counseles){
            int start = counsel.start;
            int end = counsel.end;
            //1. 해당 유형의 상담인원 중 상담 가능한 인원이 있으면 해당 상담원 배치
            if(pq.isEmpty() || pq.size() < count){
                pq.offer(new Counsel(start, end));
            }
            //2. 배치가능한 인원이 없는 경우 가장 먼저 끝나는 상담 시간과 현재 상담의 시작 시간을 비교
            else if(pq.peek().isEnd(start)){
                pq.poll();
                pq.offer(new Counsel(start, end));
            }
            //3. 해당 유형의 상담임원 중 상담 가능한 인원이 없으면 기다림
            else{
                //기다리면 앞의 상담 시간 종료 이후 바로 시작해야함
                //앞의 상담 종료 시간
                int bEnd = pq.poll().end;
                wait += bEnd - start;
                
                pq.offer(new Counsel(bEnd, bEnd+(end-start)));
            }
        }
        return wait;
    }
    
    private int sumTotalWaitTime(int[] counselors, int[][] waitTimeOfType){
        int sum = 0;
        for(int i=1;i<counselors.length;i++){
            sum += waitTimeOfType[i][counselors[i]];
        }
        return sum;
    }
}
```

### [출처]
https://school.programmers.co.kr/learn/courses/30/lessons/214288