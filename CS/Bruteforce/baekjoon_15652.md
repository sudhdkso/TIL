## N과 M (3)

### ***problem***
자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- 1부터 N까지 자연수 중에서 M개를 고른 수열
- 같은 수를 여러 번 골라도 된다.

``` java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
    public static int n; //최대 숫자?
    public static int m; //순열 개수
    public static int[] nums;
    public static StringBuilder sb = new StringBuilder();
    public static void recursive(int count){
        if(count >= m){
            for(int i=0;i<m;i++){
                sb.append(nums[i]).append(" ");

            }
            sb.append("\n");
            return;
        }

        for(int i=1;i<=n;i++){
            if(count < m){
                nums[count] = i;
                recursive(++count);
                nums[--count] = -1;
            }
        }
    }

    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String[] nm = br.readLine().split(" ");
        n = Integer.parseInt(nm[0]);
        m = Integer.parseInt(nm[1]);
        nums = new int[m];
        recursive(0);
        bw.write(String.valueOf(sb));

        bw.flush();
    }
}

```
- n은 순열을 이룰 수 있는 숫자 1부터 n까지
- m은 순열을 이루는 숫자의 개수입니다.
- nums 배열은 순열을 이루는 숫자를 저장하는 배열입니다.

- recursive라는 함수를 재귀적으로 탐색하며 새로운 순열을 찾 을 수 있습니다.
- 재귀함수를 돌며 배열에 새로운 숫자가 추가되면 count를 증가시킵니다.
- count가 m과 같아지면 순열을 이루는 숫자들를 모두 찾을 것이기 때문에 StringBuilder타입의 sb에 append해줍니다.

### 출처
https://www.acmicpc.net/problem/15651