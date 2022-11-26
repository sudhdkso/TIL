## 문자열 압축

### ***problem***
스마트폰 전화 키패드의 각 칸에 다음과 같이 숫자들이 적혀 있습니다.


이 전화 키패드에서 왼손과 오른손의 엄지손가락만을 이용해서 숫자만을 입력하려고 합니다.

맨 처음 왼손 엄지손가락은 * 키패드에 오른손 엄지손가락은 # 키패드 위치에서 시작하며, 엄지손가락을 사용하는 규칙은 다음과 같습니다.

1. 엄지손가락은 상하좌우 4가지 방향으로만 이동할 수 있으며 키패드 이동 한 칸은 거리로 1에 해당합니다.

2. 왼쪽 열의 3개의 숫자 1, 4, 7을 입력할 때는 왼손 엄지손가락을 사용합니다.

3. 오른쪽 열의 3개의 숫자 3, 6, 9를 입력할 때는 오른손 엄지손가락을 사용합니다.

4. 가운데 열의 4개의 숫자 2, 5, 8, 0을 입력할 때는 두 엄지손가락의 현재 키패드의 위치에서 더 가까운 엄지손가락을 사용합니다.

    4-1. 만약 두 엄지손가락의 거리가 같다면, 오른손잡이는 오른손 엄지손가락, 왼손잡이는 왼손 엄지손가락을 사용합니다.

순서대로 누를 번호가 담긴 배열 numbers, 왼손잡이인지 오른손잡이인 지를 나타내는 문자열 hand가 매개변수로 주어질 때, 각 번호를 누른 엄지손가락이 왼손인 지 오른손인 지를 나타내는 연속된 문자열 형태로 return 하도록 solution 함수를 완성해주세요.

[제한사항]

- numbers 배열의 크기는 1 이상 1,000 이하입니다.
- numbers 배열 원소의 값은 0 이상 9 이하인 정수입니다.
- hand는 "left" 또는 "right" 입니다.
    - "left"는 왼손잡이, "right"는 오른손잡이를 의미합니다.
- 왼손 엄지손가락을 사용한 경우는 L, 오른손 엄지손가락을 사용한 경우는 R을 순서대로 이어붙여 문자열 형태로 return 해주세요.

### ***Solution***

```java
class Solution {
        public static String calcDistance(int LH,int RH, int num){
       int[][] xy = {
                {1,0},{2,0},{3,0},
                {1,1},{2,1},{3,1},
                {1,2},{2,2},{3,2},
                {1,3},{2,3},{3,3}
        } ;

       double LD = Math.abs(xy[LH-1][0]-xy[num-1][0]) + Math.abs(xy[LH-1][1]-xy[num-1][1]);
       double RD = Math.abs(xy[RH-1][0]-xy[num-1][0]) + Math.abs(xy[RH-1][1]-xy[num-1][1]);
        
       if(LD < RD)
           return "L";
       else if(LD > RD)
           return "R";
       else
           return "LR";
    }

    public StringBuilder solution(int[] numbers, String hand) {
        StringBuilder answer = new StringBuilder();
        int LH = 10, RH = 12;
        for(int i=0; i<numbers.length; i++){
            if(numbers[i] == 1 || numbers[i] == 4 || numbers[i] == 7){
                answer.append('L');
                LH = numbers[i];
            }
            else if(numbers[i] == 3 || numbers[i] == 6 || numbers[i] == 9){
                answer.append('R');
                RH = numbers[i];
            }
            else{
                int num = 0;
                if(numbers[i] == 0)
                    num = 11;
                else
                    num = numbers[i];
                String result = calcDistance(LH,RH,num);
                if(result.equals("L")){
                    answer.append('L');
                    LH = num;
                }
                else if(result.equals("R")){
                    answer.append('R');
                    RH = num;
                }
                else{
                    if(hand.equals("left")){
                        answer.append('L');
                        LH = num;
                    }

                    else{
                        answer.append('R');
                        RH = num;
                    }

                }

            }
        }
        return answer;
    }
}
```
- **calcDistance**함수는LD(왼손의 위치)와 RD(오른손의 위치) 그리고 num(눌러야하는 숫자)을 매개변수로 받는 함수이다.
- **calcDistance**함수는 어느손이 눌러야하는 숫자와 가까운지 판별해주는 함수이다.
- **calcDistance**함수의 return 값은 `L`,`R`,`LR` 세가지 중 하나이다.
    - `L`의 경우는 왼손이 더 가깝다는 의미이다.
    - `R`의 경우는 오른손이 더 가깝다는 의미이다.
    - `LR`의 경우는 두손과의 거리가 같다는 의미이다.
- **solution**함수의 **LH**와 **RH**는 각각 왼손의 위치와 오른손의 위치를 저장하는 변수이다.
    - LH와 RH의 초기값이 각각 10,12인 이유는 시작이 `*`,`#`이기때문에 **calcDistance**함수에 매개변수로 넘겨줄 때 계산을 하기 쉽도록하기 위함이다.
- numbers 배열의 값이 **1,4,7**의 경우는 LH의 값을 numbers의 값으로 저장하고 answer에 'L'을 더한다.
- numbers 배열의 값이 **3,5,9**의 경우는 RH의 값을 numbers의 값으로 저장하고 answer에 'R'을 더한다.
- numbers 배열의 값이 **2,5,8,0**의 경우는 **calcDistance**의 결과값에 따라 LH와 RH의 값을 변경하고 answer에 값을 더한다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/67256