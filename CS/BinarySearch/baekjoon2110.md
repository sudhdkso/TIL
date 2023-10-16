## **공유기 설치**


### ***problem***
도현이의 집 N개가 수직선 위에 있다. 각각의 집의 좌표는 x1, ..., xN이고, 집 여러개가 같은 좌표를 가지는 일은 없다.

도현이는 언제 어디서나 와이파이를 즐기기 위해서 집에 공유기 C개를 설치하려고 한다. 최대한 많은 곳에서 와이파이를 사용하려고 하기 때문에, 한 집에는 공유기를 하나만 설치할 수 있고, 가장 인접한 두 공유기 사이의 거리를 가능한 크게 하여 설치하려고 한다.

C개의 공유기를 N개의 집에 적당히 설치해서, 가장 인접한 두 공유기 사이의 거리를 최대로 하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 집의 개수 N (2 ≤ N ≤ 200,000)과 공유기의 개수 C (2 ≤ C ≤ N)이 하나 이상의 빈 칸을 사이에 두고 주어진다. 둘째 줄부터 N개의 줄에는 집의 좌표를 나타내는 xi (0 ≤ xi ≤ 1,000,000,000)가 한 줄에 하나씩 주어진다.

#### **Output**
첫째 줄에 가장 인접한 두 공유기 사이의 최대 거리를 출력한다.

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
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        N = Integer.parseInt(st.nextToken());
        int C = Integer.parseInt(st.nextToken());
        arr = new int[N];

        for(int i=0;i<N;i++){
            int num = Integer.parseInt(br.readLine());
            arr[i] = num;
        }

        Arrays.sort(arr);

        int low = 1, high = arr[N-1];
        int answer = 0;
        while(low <= high){
            int mid = (low+high)/2;
            if(isCheck(C, mid)){
                answer = Math.max(answer, mid);
                low = mid + 1;
            }
            else{
                high = mid - 1;
            }
        }
        bw.write(answer+"\n");
        bw.flush();
    }

    private static boolean isCheck(int C, int threshold){

        int count = 1, n = 0;
        for(int i=1;i<N;i++){
            if(arr[n] + threshold <= arr[i]){
                count++;
                n = i;
            }
        }
        return count >= C ? true : false;
    }
}
```
### **문제 풀이**
- 이분탐색의 대상이 되는 mid는 공유기간의 거리를 의미한다.
- 이분탐색을 통해 공유기 간 거리 mid를 변화시키면서 isCheck()함수를 통해 해당 거리가 정답이 될 수 있는 지 판별한다.
    - isCheck()는 공유기 간의 거리인 threshold와 설치해야하는 공유기 갯수 C개를 매개변수로 받는다. 그리고 해당 함수는 thresold거리로 공유기를 설치할 때 C개를 모두 설치할 수 있는지 판별하여 true,false를 return하는 함수이다.
- 그리고 이분 탐색을 통해서 구하는 정답은 공유기 C개를 모두 설치할 수 있는 공유기간 최대 거리를 구해야하기 때문에 Math라이브러리의 max함수를 통해서 가능한 mid 중에서 최댓값을 answer에 갱신해준다.

- 모든 이분탐색이 끝나면 answer를 출력한다.

 
### **[출처]**
https://www.acmicpc.net/problem/2110