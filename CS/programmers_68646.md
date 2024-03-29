## **풍선 터트리기**


### ***problem***
일렬로 나열된 n개의 풍선이 있습니다. 모든 풍선에는 서로 다른 숫자가 써져 있습니다. 당신은 다음 과정을 반복하면서 풍선들을 단 1개만 남을 때까지 계속 터트리려고 합니다.

1. 임의의 인접한 두 풍선을 고른 뒤, 두 풍선 중 하나를 터트립니다.
2. 터진 풍선으로 인해 풍선들 사이에 빈 공간이 생겼다면, 빈 공간이 없도록 풍선들을 중앙으로 밀착시킵니다.

여기서 조건이 있습니다. 인접한 두 풍선 중에서 번호가 더 작은 풍선을 터트리는 행위는 최대 1번만 할 수 있습니다. 즉, 어떤 시점에서 인접한 두 풍선 중 번호가 더 작은 풍선을 터트렸다면, 그 이후에는 인접한 두 풍선을 고른 뒤 번호가 더 큰 풍선만을 터트릴 수 있습니다.

당신은 어떤 풍선이 최후까지 남을 수 있는지 알아보고 싶습니다. 위에 서술된 조건대로 풍선을 터트리다 보면, 어떤 풍선은 최후까지 남을 수도 있지만, 어떤 풍선은 무슨 수를 쓰더라도 마지막까지 남기는 것이 불가능할 수도 있습니다.

일렬로 나열된 풍선들의 번호가 담긴 배열 a가 주어집니다. 위에 서술된 규칙대로 풍선들을 1개만 남을 때까지 터트렸을 때 최후까지 남기는 것이 가능한 풍선들의 개수를 return 하도록 solution 함수를 완성해주세요.
### **제한사항**
- a의 길이는 1 이상 1,000,000 이하입니다.
    - `a[i]`는 i+1 번째 풍선에 써진 숫자를 의미합니다.
    - a의 모든 수는 -1,000,000,000 이상 1,000,000,000 이하인 정수입니다.
    - a의 모든 수는 서로 다릅니다.

### ***Solution***
``` java
class Solution {
    
    public int solution(int[] a) {
        int answer = 0;
        int index = findMinIndex(a);
        int min = Integer.MAX_VALUE;
        
        for(int i=0;i<index;i++){
            if(min > a[i]){
                min = a[i];
                answer++;
            }
        }
        min = Integer.MAX_VALUE;
        for(int i=a.length-1;i >= index ;i--){
            if(min > a[i]){
                min = a[i];
                answer++;
            }
        }
        return answer;
    }
    
    private static int findMinIndex(int[] a){
        int min = Integer.MAX_VALUE, index = 0;
        int len = a.length;
        
        for(int i=0;i<len;i++){
            if(min > a[i]){
                min = a[i];
                index = i;
            }
        }
        return index;
    }
}
```
### **문제 풀이** 
- i번째 풍선은 `[0,i-1]`구간과 `[i+1, a.length-1]` 양 구간에 모두 i번째 풍선보다 낮은 숫자가 있다면 해당 풍선을 남기는 것은 불가능 하다.
    - 그외에 한 쪽에만 i번째 풍선보다 낮은 숫자를 가진 풍선이 있다면 남기는 것은 가능하다.
- 풍선 중 최솟값을 가진 풍선의 index를 구하여 최솟 값 풍선을 기준으로 왼쪽에서 남길 수 있는 풍선과 오른쪽에서 남길 수 있는 풍선의 갯수를 구한다.
    - 중간에 최소값을 가진 풍선이 존재한다면 한 쪽만 풍선의 최솟값이 존재하는지 확인하면 된다.

- 예를 들어 풍선이 `[9, 5 , 6, -5 , 3, 4]`이라면 풍선들 중 최솟값을 가진 -5 풍선을 기준으로 왼쪽 `[9,5,6]`과 `[3,4]`로 나누어서 터트릴 수 있는 풍선의 갯수를 확인합니다.

    #### 왼쪽 `[9,5,6]`의 경우
    - 먼저 `[9,5,6]`에서는 9는 가장 왼쪽에 존재하고, 오른쪽에는 최솟값 -5가 있기 때문에 남길 수 있습니다.
        - 그리고 왼쪽 최솟값을 9로 갱신해 줍니다.
    - 5는 왼쪽에는 자신보다 큰 9가 존재 하고, 오른쪽에는 최솟값 -5가 존재하므로 남길 수 있습니다..
        - 그리고 왼쪽 최솟값을 5로 갱신해 줍니다.
    - 그리고 6은 왼쪽에 자신보다 작은 5가 존재하고, 오른쪽에도 최솟값 -5가 존재하므로 남길 수 없습니다.

    - 이렇게 왼쪽에서는 남길 수 있는 풍선은 `[9,5]` 2개입니다.

    #### 오른쪽 `[3,4]`의 경우
    - 오른쪽의 경우는 가장 마지막부터 확인합니다.
    - 먼저 4인 경우 왼쪽에는 최솟값 -5가 있고, 오른쪽에는 아무것도 존재하지 않기 때문에 남길 수 있습니다.
        - 오른쪽 최솟값을 4로 갱신해 줍니다.
    - 그리고 그 다음 3의 경우 왼쪽에 최솟값 -5가 존재하고, 오른쪽에는 자신보다 큰 4가 존재하기 때문에 남길 수 있습니다.
        - 오른쪽 최솟값을 3으로 갱신해 줍니다.
    - 그리고 마지막 가장 최솟값인 `[-5]`는 항상 양쪽이 자신보다 크기 때문에 남길 수 있습니다.
    - 이렇게 오른쪽은 최솟값을 포함하여 3개의 풍선을 남길 수 있습니다.

- 그래서 정답은 `5` 입니다.

- 이렇게 중간에 최솟값의 위치를 찾고 배열을 왼쪽과 오른쪽으로 나누고, 한쪽 방향에 항상 최솟값이 위치하게 두고 다른 한쪽 방향에 최솟값이 존재하는 지 확인하여 정답을 구할 수 있습니다.




### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/68646