## **양궁대회**


### ***problem***
카카오배 양궁대회가 열렸습니다.
`라이언`은 저번 카카오배 양궁대회 우승자이고 이번 대회에도 결승전까지 올라왔습니다. 결승전 상대는 `어피치`입니다.
카카오배 양궁대회 운영위원회는 한 선수의 연속 우승보다는 다양한 선수들이 양궁대회에서 우승하기를 원합니다. 따라서, 양궁대회 운영위원회는 결승전 규칙을 전 대회 우승자인 라이언에게 불리하게 다음과 같이 정했습니다.

1. 어피치가 화살 `n`발을 다 쏜 후에 라이언이 화살 `n`발을 쏩니다.
2. 점수를 계산합니다.
    1. 과녁판은 아래 사진처럼 생겼으며 가장 작은 원의 과녁 점수는 10점이고 가장 큰 원의 바깥쪽은 과녁 점수가 0점입니다. 
    2. 만약, k(k는 1~10사이의 자연수)점을 어피치가 a발을 맞혔고 라이언이 b발을 맞혔을 경우 더 많은 화살을 k점에 맞힌 선수가 k 점을 가져갑니다. 단, a = b일 경우는 어피치가 k점을 가져갑니다. **k점을 여러 발 맞혀도 k점 보다 많은 점수를 가져가는 게 아니고 k점만 가져가는 것을 유의하세요. 또한 a = b = 0 인 경우, 즉, 라이언과 어피치 모두 k점에 단 하나의 화살도 맞히지 못한 경우는 어느 누구도 k점을 가져가지 않습니다.**
        - 예를 들어, 어피치가 10점을 2발 맞혔고 라이언도 10점을 2발 맞혔을 경우 어피치가 10점을 가져갑니다.
        - 다른 예로, 어피치가 10점을 0발 맞혔고 라이언이 10점을 2발 맞혔을 경우 라이언이 10점을 가져갑니다.
    3. 모든 과녁 점수에 대하여 각 선수의 최종 점수를 계산합니다.
3. 최종 점수가 더 높은 선수를 우승자로 결정합니다. 단, 최종 점수가 같을 경우 어피치를 우승자로 결정합니다.

현재 상황은 어피치가 화살 `n`발을 다 쏜 후이고 라이언이 화살을 쏠 차례입니다.

라이언은 어피치를 가장 큰 점수 차이로 이기기 위해서 `n`발의 화살을 어떤 과녁 점수에 맞혀야 하는지를 구하려고 합니다.

화살의 개수를 담은 자연수 `n`, 어피치가 맞힌 과녁 점수의 개수를 10점부터 0점까지 순서대로 담은 정수 배열 `info`가 매개변수로 주어집니다. 이때, 라이언이 가장 큰 점수 차이로 우승하기 위해 `n`발의 화살을 어떤 과녁 점수에 맞혀야 하는지를 10점부터 0점까지 순서대로 정수 배열에 담아 return 하도록 solution 함수를 완성해 주세요. 만약, 라이언이 우승할 수 없는 경우(무조건 지거나 비기는 경우)는 `[-1]`을 return 해주세요.

### **제한 사항**
- 1 ≤ `n` ≤ 10
- `info`의 길이 = 11
    - 0 ≤ `info`의 원소 ≤ `n`
    - `info`의 원소 총합 = `n`
    - `info`의 i번째 원소는 과녁의 `10 - i` 점을 맞힌 화살 개수입니다. ( i는 0~10 사이의 정수입니다.)
- 라이언이 우승할 방법이 있는 경우, return 할 정수 배열의 길이는 11입니다.
    - 0 ≤ return할 정수 배열의 원소 ≤ `n`
    - return할 정수 배열의 원소 총합 = `n` (꼭 n발을 다 쏴야 합니다.)
    - return할 정수 배열의 i번째 원소는 과녁의 `10 - i` 점을 맞힌 화살 개수입니다. ( i는 0~10 사이의 정수입니다.)
    - **라이언이 가장 큰 점수 차이로 우승할 수 있는 방법이 여러 가지 일 경우, 가장 낮은 점수를 더 많이 맞힌 경우를 return 해주세요.**
    - 가장 낮은 점수를 맞힌 개수가 같을 경우 계속해서 그다음으로 낮은 점수를 더 많이 맞힌 경우를 return 해주세요.
    - 예를 들어, `[2,3,1,0,0,0,0,1,3,0,0]`과 `[2,1,0,2,0,0,0,2,3,0,0]`를 비교하면 `[2,1,0,2,0,0,0,2,3,0,0]`를 return 해야 합니다.
    - 다른 예로, `[0,0,2,3,4,1,0,0,0,0,0]`과 `[9,0,0,0,0,0,0,0,1,0,0]`를 비교하면`[9,0,0,0,0,0,0,0,1,0,0]`를 return 해야 합니다.
- 라이언이 우승할 방법이 없는 경우, return 할 정수 배열의 길이는 1입니다.
    - 라이언이 어떻게 화살을 쏘든 **라이언의 점수가 어피치의 점수보다 낮거나 같으면 `[-1]을` return 해야 합니다.**

### ***Solution***
``` java
import java.util.Arrays;
class Solution {
    private static int LEN = 12;
    private static int[] maxScore = new int[LEN];
    private static int max = 0;
    public int[] solution(int n, int[] info) {
        int[] answer = {};

        dfs(info, 0, 0,n, new int[LEN]);
        if(max == 0){
            return new int[]{-1};
        }
        return maxScore;
    }
    
    private static void dfs(int[] info, int depth, int count, int n, int[] score){
        if(count > n) return;
        if(depth == LEN-1 || count == n){
            score[10] = n - count;
            
            if(checkScore(info, score) ){
                maxScore = Arrays.copyOf(score, 11);
            }
            return;
        }
        if(n >= count + info[depth] + 1){
            score[depth] = info[depth] + 1;
            dfs(info, depth+1, count+ (info[depth] + 1), n,score);    
        }
        score[depth] = 0;
        dfs(info, depth+1, count, n, score);  
    }
    
    private static boolean checkScore(int[] info, int[] score){
        int apeach = 0, ryan = 0;
        for(int i=0;i<LEN-1;i++){
            if(info[i] == 0 && score[i] == 0) continue;
            if(info[i] >= score[i]){
                apeach += 10 - i;
            }
            else{
                ryan += 10 - i;
            }
        }
        
        int diff = ryan - apeach;
        if(apeach > ryan || max > diff){
            return false;
        }
        
        if(diff > max){
            max = diff;
            return true;
        }

        for(int i=10;i>=0;i--){
            if(score[i] == 0 && maxScore[i] == 0) continue;
            if(score[i] > maxScore[i]){
                return true;
            }
            else if(score[i] < maxScore[i]){
                return false;
            }
        }
        
        return true;
        
    }
}
```
### **문제 풀이** 
- dfs를 활용하여 라이언이 맞출 수 있는 경우의 수를 모두 구하여, 각 경우의 수에서 어피치와 최대 점수차이가 얼마인지 확인한다.

- dfs를 활용하여 두가지 경우의 수로 재귀함수를 돈다.
    1. 현재 맞추려는 과녁이 어피치가 맞춘 과녁보다 1개 더 많이 맞출 때
    2. 그냥 해당 점수 과녁을 맞추지 않을 때

- 1번의 경우 먼저 화살의 갯수를 맞춘 만큼 증가시킨다.

- 그리고 마지막 0점까지 재귀함수를 돌았던가, 아니면 화살을 모두 과녁에 맞춘 경우에 재귀함수를 멈춘다.
    - 만약 모든 화살을 과녁에 맞추지 않은 경우에는 `0점`과녁에 모든 화살을 맞춘것으로 간주한다.
        - 정답이 여러개의 경우 낮은 점수의 과녁을 많이 맞춘 경우의 수를 return해줘야하기 떄문
- 그리고 `checkScore()`함수를 통해, 어피치에게 이겼는지 먼저 확인한다.
    #### `checkScore()`함수
    - `checkScore()`함수에서는 어피치의 점수보다 낮은 경우와 이미 구해놓은 어피치와의 점수차이 보다 현재 점수차이가 작은 경우 false를 return한다.
    - 그리고 현재점수 차이가 그전 점수차이보다 크면 true를 return한다.
    - 마지막으로 점수차이가 같은 경우에는 낮은 과녁을 더 많이 맞추면 true를 return하고, 그게 아니면 false를 return한다.
- 그렇게 `checkScore()`의 반환결과가 true인 경우 maxScore배열을 현재 score배열의 값으로 깊은 복사 해준다.

- 이렇게 dfs를 통해 모든 경우의 수를 확인한 후 maxScore배열을 정답으로 return해준다.
    




### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/92342