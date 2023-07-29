## **유사 칸토어 비트열**


### ***problem***
수학에서 칸토어 집합은 0과 1 사이의 실수로 이루어진 집합으로, [0, 1]부터 시작하여 각 구간을 3등분하여 가운데 구간을 반복적으로 제외하는 방식으로 만들어집니다.

남아는 칸토어 집합을 조금 변형하여 유사 칸토어 비트열을 만들었습니다. 유사 칸토어 비트열은 다음과 같이 정의됩니다.

- 0 번째 유사 칸토어 비트열은 "1" 입니다.
- n(1 ≤ n) 번째 유사 칸토어 비트열은 n - 1 번째 유사 칸토어 비트열에서의 1을 11011로 치환하고 0을 00000로 치환하여 만듭니다.

남아는 `n` 번째 유사 칸토어 비트열에서 특정 구간 내의 1의 개수가 몇 개인지 궁금해졌습니다.
`n`과 1의 개수가 몇 개인지 알고 싶은 구간을 나타내는 `l`, `r`이 주어졌을 때 그 구간 내의 1의 개수를 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- 1 ≤ `n` ≤ 20
- 1 ≤ `l`, `r` ≤ $5^n$
    - l ≤ `r` < `l` + 10,000,000
    - `l`과 `r`은 비트열에서의 인덱스(1-base)이며 폐구간 [l, r]을 나타냅니다.

### ***Solution***
``` java
import java.util.Arrays;
class Solution {
    public long solution(int n, long l, long r) {
        int answer = 0;
        long[] cantorLen = getCantorLen(n);
        long[] cantorOne = getCantorOne(n);
        long lCount = getOneByRange(cantorLen, cantorOne, n, l-1);
        long rCount = getOneByRange(cantorLen, cantorOne, n, r);
        return rCount-lCount;
    }
    
    private static long[] getCantorLen(int n){
        long[] cantorLen = new long[n+1];
        cantorLen[0] = 1;
        for(int i=1;i<=n;i++){
            cantorLen[i] = cantorLen[i-1]*5;
        }
        return cantorLen;
    }
    
    private static long[] getCantorOne(int n){
        long[] cantorOne = new long[n+1];
        cantorOne[0] = 1;
        for(int i=1;i<=n;i++){
            cantorOne[i] = cantorOne[i-1]*4;
        }
        return cantorOne;
    }
    
    private static long getOneByRange(long[] cantorLen, long[] cantorOne, int n, long e){
        //11011
        int index = n;
        long count = 0, q = 0 , remain = e;
        int[] arr = {0,1,2,2,3};
        while(index-- > 1){
            q = remain/cantorLen[index];
            remain = remain%cantorLen[index];
            count += q * cantorOne[index];
            if(q == 2){
                return count;
            }
            if(q >= 3){
                count -= cantorOne[index];
            }
            
        }

        if(remain == 0) return count;
        
        return count + arr[(int)remain];
    }
}
```
### **문제 풀이** 
- 1과 0을 각각 11011, 00000으로 치환하며 5의 거듭제곱만큼씩 길이가 증가한다.
- 따라서 실제로 치환하여 문제를 푸는 것은 시간 초과가 날 가능성이 높다.

- 문제에서 폐구간 [l,r]사이에 1의 갯수가 몇개 들어가는지 확인해야 하므로, 1의 갯수를 먼저 파악해야한다.
    - 1의 갯수는 n=0일때 1개 n=1일때 (11011)이므로 4개, n=2일때는 16개로 4의 거듭제곱개 만큼 늘어난다.
    - 주어진 구간내의 1의 갯수는 `[1,r]사이의 1의 갯수` - `[1,l-1]사이의 1의 갯수`로 구할 수 있다.

- 그리고 문제에서는 n-1개의 유사 칸토어 비트열이 주어진 구간에 몇개가 포함되어 있는지 확인할 수 있다.
    - 예를 들어 문제에서 
        - n=2이고, l=4이고 r=17일 때
    - 유사 칸토어 비트열은 `11011 11011 0000 11011 11011`이다.
    - [1,17]까지의 1의 갯수는 `17/5`한 3개의 n-1칸토어 비트열과 나머지 2개로 구할 수 표현할 수 있다.
    - 따라서 3개의 n-1칸토어 비트열에 포함된 1의 갯수는 3 * $4^1$인 `12`가 되는 것 같지만, 3번째 묶음은 `00000`이기 때문에 2 * $4^1$인 8개가 있다고 생각할 수 있다.
    - 그리고 나머지 2에 대해서는 11011의 2번째까지 포함되기 때문에 위의 값에 2를 더한다.
    - 위와 같은 방식으로 [1,l-1]구간의 1의 갯수를 구해서 서로 뺀다.
- 위와 같은 방식으로 1의 갯수를 구하면서 칸토어 비트열의 구간을 5단위로 계속 나누어 가면서 구한다.




### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/148652#