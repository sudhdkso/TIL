# Java 1D Array
### 문제
>  java의 1차원 배열을 이용하는 문제, 첫줄에 입력받는 n은 배열의 크기고 배열에 정수를 입력받아 한줄씩 출력


### ***Solution***

```java
import java.util.*;

public class Solution {

    public static void main(String[] args) {
	   
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt();
         int[] a=new int[n];
        for(int i=0;i<a.length;i++){
            int num=scan.nextInt();
            a[i]=num;
        }
        scan.close();

        // Prints each sequential element in array a
        for (int i = 0; i < a.length; i++) {
            System.out.println(a[i]);
        }
    }
}
```