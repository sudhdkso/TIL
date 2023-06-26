## **이모티콘 할인행사**


#### ***problem***
카카오톡에서는 이모티콘을 무제한으로 사용할 수 있는 이모티콘 플러스 서비스 가입자 수를 늘리려고 합니다.
이를 위해 카카오톡에서는 이모티콘 할인 행사를 하는데, 목표는 다음과 같습니다.

1. 이모티콘 플러스 서비스 가입자를 최대한 늘리는 것.
2. 이모티콘 판매액을 최대한 늘리는 것.

<u>**1번 목표가 우선이며, 2번 목표가 그 다음입니다.**</u>

이모티콘 할인 행사는 다음과 같은 방식으로 진행됩니다.

- `n`명의 카카오톡 사용자들에게 이모티콘 `m`개를 할인하여 판매합니다.
- 이모티콘마다 할인율은 다를 수 있으며, 할인율은 10%, 20%, 30%, 40% 중 하나로 설정됩니다.

카카오톡 사용자들은 다음과 같은 기준을 따라 이모티콘을 사거나, 이모티콘 플러스 서비스에 가입합니다.

- 각 사용자들은 자신의 기준에 따라 일정 비율 이상 할인하는 이모티콘을 모두 구매합니다.
- 각 사용자들은 자신의 기준에 따라 이모티콘 구매 비용의 합이 일정 가격 이상이 된다면, 이모티콘 구매를 모두 취소하고 이모티콘 플러스 서비스에 가입합니다.

다음은 2명의 카카오톡 사용자와 2개의 이모티콘이 있을때의 예시입니다.

|사용자	|비율	|가격|
|---|---|---|
|1	|40	|10,000|
|2	|25	|10,000|

|이모티콘|	가격|
|---|---|
|1	|7,000|
|2	|9,000|

1번 사용자는 40%이상 할인하는 이모티콘을 모두 구매하고, 이모티콘 구매 비용이 10,000원 이상이 되면 이모티콘 구매를 모두 취소하고 이모티콘 플러스 서비스에 가입합니다.

2번 사용자는 25%이상 할인하는 이모티콘을 모두 구매하고, 이모티콘 구매 비용이 10,000원 이상이 되면 이모티콘 구매를 모두 취소하고 이모티콘 플러스 서비스에 가입합니다.

1번 이모티콘의 가격은 7,000원, 2번 이모티콘의 가격은 9,000원입니다.

만약, 2개의 이모티콘을 모두 40%씩 할인한다면, 1번 사용자와 2번 사용자 모두 1,2번 이모티콘을 구매하게 되고, 결과는 다음과 같습니다.

|사용자	|구매한 이모티콘	|이모티콘 구매 비용|	이모티콘 플러스 서비스 가입 여부|
|---|---|---|---|
|1	|1, 2|	9,600|	X|
|2	|1, 2|	9,600|	X|

이모티콘 플러스 서비스 가입자는 0명이 늘어나고 이모티콘 판매액은 19,200원이 늘어납니다.

하지만, 1번 이모티콘을 30% 할인하고 2번 이모티콘을 40% 할인한다면 결과는 다음과 같습니다.

|사용자	|구매한 이모티콘	|이모티콘 구매 비용|	이모티콘 플러스 서비스 가입 여부|
|---|---|---|---|
|1	|2|	5,400|	X|
|2	|1, 2|	9,600|	O|

2번 사용자는 이모티콘 구매 비용을 10,000원 이상 사용하여 이모티콘 구매를 모두 취소하고 이모티콘 플러스 서비스에 가입하게 됩니다.
따라서, 이모티콘 플러스 서비스 가입자는 1명이 늘어나고 이모티콘 판매액은 5,400원이 늘어나게 됩니다.

카카오톡 사용자 `n`명의 구매 기준을 담은 2차원 정수 배열 `users`, 이모티콘 `m`개의 정가를 담은 1차원 정수 배열 `emoticons`가 주어집니다. 이때, 행사 목적을 최대한으로 달성했을 때의 이모티콘 플러스 서비스 가입 수와 이모티콘 매출액을 1차원 정수 배열에 담아 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- 1 ≤ `users`의 길이 = `n` ≤ 100
    - `users`의 원소는 [`비율`, `가격`]의 형태입니다.
    - `users[i]`는 `i+1`번 고객의 구매 기준을 의미합니다.
    - `비율`% 이상의 할인이 있는 이모티콘을 모두 구매한다는 의미입니다.
        - 1 ≤ `비율` ≤ 40
    - `가격`이상의 돈을 이모티콘 구매에 사용한다면, 이모티콘 구매를 모두 취소하고 이모티콘 플러스 서비스에 가입한다는 의미입니다.
        - 100 ≤ `가격` ≤ 1,000,000
        - `가격`은 100의 배수입니다.
- 1 ≤ `emoticons`의 길이 = `m` ≤ 7
    - `emoticons[i]`는 `i+1`번 이모티콘의 정가를 의미합니다.
    - 100 ≤ `emoticons`의 원소 ≤ 1,000,000
    - `emoticons`의 원소는 100의 배수입니다.-

### ***Solution***
``` java
import java.util.*;
class Solution {
    static int[] arr;
    static List<Integer[]> list = new ArrayList<>();
    public int[] solution(int[][] users, int[] emoticons) {
        int[] answer = new int[2];
        Arrays.sort(emoticons);
        
        arr = new int[emoticons.length];
        
        getAllSale(-1,10,emoticons);
        int index = 0;
        
        for(Integer[] sale : list){
            int count = 0, maxMoney = 0;
            for(int i=0;i<users.length;i++){
                int result = calcEmoticon(users[i],emoticons,sale);
                if(result < 0){
                    count++;
                }
                else{
                    maxMoney += result;
                }               
            }
            if(answer[0] < count){
                answer[0] = count;
                answer[1] = maxMoney;
                
            }
            if(answer[0] == count && answer[1] < maxMoney){
                answer[1] = maxMoney;
            }
        }
        return answer;
    }
    
    private static int calcEmoticon(int[] user, int[] emoticons, Integer[] sale){
        int len = emoticons.length;
        int wantSale = user[0], money = 0;
        for(int i=0;i<len;i++){
            if(wantSale <= sale[i]){
                money +=  emoticons[i]*((100-sale[i])*0.01);
            }
        }
        if(money >= user[1]){
            return -1;
        }
        return money;
    }
    
    private static void getAllSale(int index, int sum, int[] emoticons){
        if(index >= emoticons.length){
            Integer[] temp = Arrays.stream(arr).boxed().toArray(Integer[]::new);
            if(!isInList(temp)){
                list.add(temp);
            }
            return;
        }
        if(index >= 0){
            arr[index] = sum;
        }
        
        for(int i=10;i<=40;i+=10){
            getAllSale(index+1, i, emoticons);         
        }
    }
    
    public static boolean isInList(final Integer[] candidate) {
        return list.stream().anyMatch(s -> Arrays.equals(s, candidate));
    }
}
```
- `getAllSale()`은 재귀함수를 통해서 이모티콘들의 할인 비율의 모든 경우의 수를 계한하여 list에 넣는 함수이다.
- `isInList()`는 list에 해당 배열이 있는지 확인하는 함수이다.
- `calcEmoticon()`은 각 user별로 매개변수로 받은 sale을 통해서 구매할 수 있는 이모티콘들을 모두 구매할 때 가격을 계산하여 return해주는 함수이다.
    - 이때 return값이 -1이면 이모티콘 플러스를 사는 고객이라는 의미이다.


### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/150368