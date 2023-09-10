## **등산코스 정하기**


### ***problem***
민호는 다단계 조직을 이용하여 칫솔을 판매하고 있습니다. 판매원이 칫솔을 판매하면 그 이익이 피라미드 조직을 타고 조금씩 분배되는 형태의 판매망입니다. 어느정도 판매가 이루어진 후, 조직을 운영하던 민호는 조직 내 누가 얼마만큼의 이득을 가져갔는지가 궁금해졌습니다. 예를 들어, 민호가 운영하고 있는 다단계 칫솔 판매 조직이 아래 그림과 같다고 합시다.
<p><img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/69c07bd8-1707-422c-a05d-5de3498b7048/%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B71.png" title="" alt="그림1.png"></p>

민호는 center이며, 파란색 네모는 여덟 명의 판매원을 표시한 것입니다. 각각은 자신을 조직에 참여시킨 추천인에 연결되어 피라미드 식의 구조를 이루고 있습니다. 조직의 이익 분배 규칙은 간단합니다. 모든 판매원은 칫솔의 판매에 의하여 발생하는 이익에서 10% 를 계산하여 자신을 조직에 참여시킨 추천인에게 배분하고 나머지는 자신이 가집니다. 모든 판매원은 자신이 칫솔 판매에서 발생한 이익 뿐만 아니라, 자신이 조직에 추천하여 가입시킨 판매원에게서 발생하는 이익의 10% 까지 자신에 이익이 됩니다. 자신에게 발생하는 이익 또한 마찬가지의 규칙으로 자신의 추천인에게 분배됩니다. 단, 10% 를 계산할 때에는 원 단위에서 절사하며, 10%를 계산한 금액이 1 원 미만인 경우에는 이득을 분배하지 않고 자신이 모두 가집니다.

예를 들어, 아래와 같은 판매 기록이 있다고 가정하겠습니다. 칫솔의 판매에서 발생하는 이익은 개당 100 원으로 정해져 있습니다.

<table class="table">
        <thead><tr>
<th>판매원</th>
<th>판매 수량</th>
<th>이익금</th>
</tr>
</thead>
        <tbody><tr>
<td>young</td>
<td>12</td>
<td>1,200 원</td>
</tr>
<tr>
<td>john</td>
<td>4</td>
<td>400 원</td>
</tr>
<tr>
<td>tod</td>
<td>2</td>
<td>200 원</td>
</tr>
<tr>
<td>emily</td>
<td>5</td>
<td>500 원</td>
</tr>
<tr>
<td>mary</td>
<td>10</td>
<td>1,000 원</td>
</tr>
</tbody>
      </table>

판매원 young 에 의하여 1,200 원의 이익이 발생했습니다. young 은 이 중 10% 에 해당하는 120 원을, 자신을 조직에 참여시킨 추천인인 edward 에게 배분하고 자신은 나머지인 1,080 원을 가집니다. edward 는 young 에게서 받은 120 원 중 10% 인 12 원을 mary 에게 배분하고 자신은 나머지인 108 원을 가집니다. 12 원을 edward 로부터 받은 mary 는 10% 인 1 원을 센터에 (즉, 민호에게) 배분하고 자신은 나머지인 11 원을 가집니다. 이 상태를 그림으로 나타내면 아래와 같습니다.

<p><img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/f016005d-6555-4c05-ad39-b413645b9217/%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B72.png" title="" alt="그림2.png"></p>

그 후, 판매원 john 에 의하여 400 원의 이익이 발생합니다. john 은 10% 인 40 원을 센터에 배분하고 자신이 나머지인 360 원을 가집니다. 이 상태를 그림으로 나타내면 아래와 같습니다.

<p><img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/89418fb8-a704-4856-81e2-f84038d71ee2/%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B73.png" title="" alt="그림3.png"></p>

또 그 후에는 판매원 tod 에 의하여 200 원 이익이 발생하는데, tod 자신이 180 원을, 추천인인 jaimie 가 그 중 10% 인 20 원을 받아서 18 원을 가지고, jaimie 의 추천인인 mary 는 2 원을 받지만 이것의 10% 는 원 단위에서 절사하면 배분할 금액이 없기 때문에 mary 는 2 원을 모두 가집니다. 이 상태를 그림으로 나타내면 아래와 같습니다.

<p><img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/4fb2164f-71b0-48ff-a2d0-e2fd8133e329/%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B75.png" title="" alt="그림5.png"></p>

마지막으로, 판매원 mary 는 1,000 원의 이익을 달성하고, 이 중 10% 인 100 원을 센터에 배분한 후 그 나머지인 900 원을 자신이 가집니다. 이 상태를 그림으로 나타내면 아래와 같습니다.

<p><img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/0527a713-4fc4-47db-98d3-49ce6d911dfd/%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B76.png" title="" alt="그림6.png"></p>

위와 같이 하여 모든 조직 구성원들의 이익 달성 현황 집계가 끝났습니다. 지금까지 얻은 이익을 모두 합한 결과를 그림으로 나타내면 아래와 같습니다.

<p><img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/970f1df0-4f00-480f-93a3-13c7e30b19cb/%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B77.png" title="" alt="그림7.png"></p>

이 결과가 민호가 파악하고자 하는 이익 배분 현황입니다.

각 판매원의 이름을 담은 배열 enroll, 각 판매원을 다단계 조직에 참여시킨 다른 판매원의 이름을 담은 배열 referral, 판매량 집계 데이터의 판매원 이름을 나열한 배열 seller, 판매량 집계 데이터의 판매 수량을 나열한 배열 amount가 매개변수로 주어질 때, 각 판매원이 득한 이익금을 나열한 배열을 return 하도록 solution 함수를 완성해주세요. 판매원에게 배분된 이익금의 총합을 계산하여(정수형으로), 입력으로 주어진 enroll에 이름이 포함된 순서에 따라 나열하면 됩니다.

### **제한 사항**
- enroll의 길이는 1 이상 10,000 이하입니다.
    - enroll에 민호의 이름은 없습니다. 따라서 enroll의 길이는 민호를 제외한 조직 구성원의 총 수입니다.
- referral의 길이는 enroll의 길이와 같습니다.
    - referral 내에서 i 번째에 있는 이름은 배열 enroll 내에서 i 번째에 있는 판매원을 조직에 참여시킨 사람의 이름입니다.
    - 어느 누구의 추천도 없이 조직에 참여한 사람에 대해서는 referral 배열 내에 추천인의 이름이 기입되지 않고 “-“ 가 기입됩니다. 위 예제에서는 john 과 mary 가 이러한 예에 해당합니다.
    - enroll 에 등장하는 이름은 조직에 참여한 순서에 따릅니다.
    - 즉, 어느 판매원의 이름이 enroll 의 i 번째에 등장한다면, 이 판매원을 조직에 참여시킨 사람의 이름, 즉 referral 의 i 번째 원소는 이미 배열 enroll 의 j 번째 (j < i) 에 등장했음이 보장됩니다.
- seller의 길이는 1 이상 100,000 이하입니다.
    - seller 내의 i 번째에 있는 이름은 i 번째 판매 집계 데이터가 어느 판매원에 의한 것인지를 나타냅니다.
    - seller 에는 같은 이름이 중복해서 들어있을 수 있습니다.
- amount의 길이는 seller의 길이와 같습니다.
    - amount 내의 i 번째에 있는 수는 i 번째 판매 집계 데이터의 판매량을 나타냅니다.
    - 판매량의 범위, 즉 amount 의 원소들의 범위는 1 이상 100 이하인 자연수입니다.
- 칫솔 한 개를 판매하여 얻어지는 이익은 100 원으로 정해져 있습니다.
- 모든 조직 구성원들의 이름은 10 글자 이내의 영문 알파벳 소문자들로만 이루어져 있습니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    private static class Node{
        Node parent;
        String name;
        
        public Node(Node parent, String name){
            this.parent = parent;
            this.name = name;
        }
        
        public Node(String name){
            this(null, name);
        }
        
        @Override
        public boolean equals(Object obj){
            Node e = (Node)obj;
            if(e.name.equals(this.name)){
                return true;
            }
            return false;
        }
        
        @Override
        public String toString(){
            return "["+this.name+"] \n";
        }
    }
    
    static int[] result;
    static List<Node> group = new ArrayList<>();
    static Map<String,Integer> map = new HashMap<>();
    public int[] solution(String[] enroll, String[] referral, String[] seller, int[] amount) {
        int n = enroll.length;
        int[] answer = new int[n];
        result = new int[n];
        
        for(int i=0;i<n;i++){
            if(referral[i].equals("-")){
                group.add(new Node(enroll[i]));
                map.put(enroll[i],i);
                continue;
            }
            int index = map.get(referral[i]);

            Node parent = group.get(index);
            
            group.add(new Node(parent, enroll[i]));
            map.put(enroll[i],i);
        }

        for(int i=0;i<seller.length;i++){
            setProfit(seller[i],amount[i]);
        }

        return result;
    }
    
    private static void setProfit(String seller, int amount){
        int money = amount*100;
        while(money > 0){
            int index = map.get(seller);
            Node now = group.get(index);
            if(money/10 <= 0){
                result[index] += money;
            }
            else{
                result[index] += money-(money/10);    
            }

            money/=10;
            if(now.parent == null){
                break;
            }
            seller = now.parent.name;
        }
    }
}
```
### **문제 풀이** 
- 



### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/77486