## **용액**


### ***problem***
첫째 줄에는 전체 용액의 수 N이 입력된다. N은 2 이상 100,000 이하의 정수이다. 둘째 줄에는 용액의 특성값을 나타내는 N개의 정수가 빈칸을 사이에 두고 오름차순으로 입력되며, 이 수들은 모두 -1,000,000,000 이상 1,000,000,000 이하이다. N개의 용액들의 특성값은 모두 서로 다르고, 산성 용액만으로나 알칼리성 용액만으로 입력이 주어지는 경우도 있을 수 있다.

#### **Output**
첫째 줄에 특성값이 0에 가장 가까운 용액을 만들어내는 두 용액의 특성값을 출력한다. 출력해야 하는 두 용액은 특성값의 오름차순으로 출력한다. 특성값이 0에 가장 가까운 용액을 만들어내는 경우가 두 개 이상일 경우에는 그 중 아무것이나 하나를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {

    static int[] arr;
    static int N;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        N = Integer.parseInt(br.readLine());

        arr = new int[N];

        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        for(int i=0;i<N;i++){
            arr[i] = Integer.parseInt(st.nextToken());
        }

        Arrays.sort(arr);

        int left = 0, right = N-1;

        int min = 2_000_000_001, a = 0, b = 0;

        while(left < right && left >=0 && right >= 0 && left < N && right < N){
            int num = arr[left] + arr[right];
            if(isCheck(num, min)){
                min = num;
                a = arr[left];
                b = arr[right];
            }

            if(num > 0){
                right -= 1;
            }
            else if(num < 0){
                left += 1;
            }
            else{
                break;
            }
        }

        bw.write(a+" "+b+"\n");
        bw.flush();
    }

    private static boolean isCheck(int a, int b){
        return Math.abs(a) < Math.abs(b) ? true : false;
    }
}
```
### **문제 풀이**

 
### **[출처]**
https://www.acmicpc.net/problem/2467