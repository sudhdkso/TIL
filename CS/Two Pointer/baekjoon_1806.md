### **부분합**


#### ***problem***
10,000 이하의 자연수로 이루어진 길이 N짜리 수열이 주어진다. 이 수열에서 연속된 수들의 부분합 중에 그 합이 S 이상이 되는 것 중, 가장 짧은 것의 길이를 구하는 프로그램을 작성하시오.

#### Input
첫째 줄에 N (10 ≤ N < 100,000)과 S (0 < S ≤ 100,000,000)가 주어진다. 둘째 줄에는 수열이 주어진다. 수열의 각 원소는 공백으로 구분되어져 있으며, 10,000이하의 자연수이다.

#### Output
첫째 줄에 구하고자 하는 최소의 길이를 출력한다. 만일 그러한 합을 만드는 것이 불가능하다면 0을 출력하면 된다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    public static int[] CalcPrefixSum(int[] arr){
        int length = arr.length;
        int[] prefixSum = new int[length+1];
        prefixSum[0] = 0;
        for(int i=1;i<length+1;i++){
            prefixSum[i] = prefixSum[i-1] + arr[i-1];
        }
        return prefixSum;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String[] NS = br.readLine().split(" ");
        int n = Integer.parseInt(NS[0]);
        int s = Integer.parseInt(NS[1]);

        int[] arr = new int[n];
        String[] nums = br.readLine().split(" ");

        for(int i=0;i<n;i++){
            arr[i] = Integer.parseInt(nums[i]);
        }

        int[] prefixSum = CalcPrefixSum(arr);

        int left = 0,right = 0, sum = 0;
        int min = Integer.MAX_VALUE;
        while( right >= left && right <= n){
            sum = prefixSum[right] - prefixSum[left];
            if(sum >= s){
                min = Math.min(min, right - left);
                left++;
            }
            else if(sum < s){
                right++;
            }
        }
        min = min == Integer.MAX_VALUE ? 0 : min;
        bw.write(min +"\n");
        bw.flush();
    }

}
```
- calcPrefixSum arr의 누적합 계산하여 int형 일차원 배열을 return해주는 함수이다.

### 출처
https://www.acmicpc.net/problem/1806
