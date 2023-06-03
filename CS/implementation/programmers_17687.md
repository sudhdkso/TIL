## **N진수 게임**


#### ***problem***
N진수 게임
튜브가 활동하는 코딩 동아리에서는 전통적으로 해오는 게임이 있다. 이 게임은 여러 사람이 둥글게 앉아서 숫자를 하나씩 차례대로 말하는 게임인데, 규칙은 다음과 같다.

1. 숫자를 0부터 시작해서 차례대로 말한다. 첫 번째 사람은 0, 두 번째 사람은 1, … 열 번째 사람은 9를 말한다.
2. 10 이상의 숫자부터는 한 자리씩 끊어서 말한다. 즉 열한 번째 사람은 10의 첫 자리인 1, 열두 번째 사람은 둘째 자리인 0을 말한다.

이렇게 게임을 진행할 경우,
`0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 1, 0, 1, 1, 1, 2, 1, 3, 1, 4, …`
순으로 숫자를 말하면 된다.

한편 코딩 동아리 일원들은 컴퓨터를 다루는 사람답게 이진수로 이 게임을 진행하기도 하는데, 이 경우에는
`0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, …`
순으로 숫자를 말하면 된다.

이진수로 진행하는 게임에 익숙해져 질려가던 사람들은 좀 더 난이도를 높이기 위해 이진법에서 십육진법까지 모든 진법으로 게임을 진행해보기로 했다. 숫자 게임이 익숙하지 않은 튜브는 게임에 져서 벌칙을 받는 굴욕을 피하기 위해, 자신이 말해야 하는 숫자를 스마트폰에 미리 출력해주는 프로그램을 만들려고 한다. 튜브의 프로그램을 구현하라.

#### **입력형식**
진법 `n`, 미리 구할 숫자의 갯수 `t`, 게임에 참가하는 인원 `m`, 튜브의 순서 `p` 가 주어진다.

- 2 ≦ `n` ≦ 16
- 0 ＜ `t` ≦ 1000
- 2 ≦ `m` ≦ 100
- 1 ≦ `p` ≦ `m`

#### **출력형식**
튜브가 말해야 하는 숫자 `t`개를 공백 없이 차례대로 나타낸 문자열. 단, `10`~`15`는 각각 대문자 `A`~`F`로 출력한다.

### ***Solution***
``` java
class Solution {
    public static String convertNRadix(int n, int num){
        StringBuilder sb = new StringBuilder();
        while(num > 0){
            if(n > 10 && num%n >= 10){
                sb.append((char)(55+(num%n)));
            }
            else sb.append(num%n);
            num/=n;
        }
        return sb.reverse().toString();
    }
    public static String getConvertNradixAll(int n, int t, int m){
        StringBuilder sb = new StringBuilder("0");
        int num = 0;
        while(true){
            if(sb.length() >= m*t) break;
            sb.append(convertRadix(n,++num));
        }
        return sb.toString();
    }
    
    public String solution(int n, int t, int m, int p) {
        String answer = "";
        StringBuilder sb = new StringBuilder();
        String radixAll = getConvertRadixAll(n,t,m);
        int order = 0;
        for(int i=0;i<t*m; i++){
            order = order >= m ? 1 : order+1;
            if(order == p){
                sb.append(radixAll.substring(i,i+1));
            }
        }
        
        return sb.toString();
    }
}
```
- convertNRadix()함수는 num을 N진수로 변환하여 반환하는 함수이다.
- getConvertNradixAll()함수는 튜브가 필요한 숫자만큼 진수를 미리 확보하기 위한 함수이다.
    - 튜브가 필요한 미리 구할 숫자의 갯수 t이다.
    - 그리고 게임 참여인원은 m이기 때문에 미리 구해놓아야하는 숫자의 최대값은 t*m이된다.
    - 따라서 이 함수는 0부터 ~ x까지 숫자를 증가시켜 N진법으로 변환한 값을 sb에 append하며 sb의 길이가 t*m이상이 될때가지 구하는 함수이다.

- getConvertNradixAll()함수를 통해 미리 필요한 문자열을 구하여 radixAll에 저장한다.
- 그리고 order변수를 증가시키며 order가 p(튜브의 순서)와 같으면 sb에 해당 순서의 radixAll을 append한다.
- order가 m(참여인원)과 같아지면 order를 다시 1번으로 초기화한다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/17687