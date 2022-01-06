# JavaBigInteger
### 문제
>  매우 큰 수를 더하고 곱하기 위해서 Java의 BigInteger클래스의 기능을 사용


### ***Solution***

```java
import java.io.*;
import java.util.*;
import java.math.*;

public class Solution {

    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        BigInteger b1 = new BigInteger(input.nextBigInteger().toString());
        BigInteger b2 = new BigInteger(input.nextBigInteger().toString());

        System.out.println(b1.add(b2));
        System.out.println(b1.multiply(b2));

    }
}
```

- BigInteger는 문자열로 되어있기 때문에 입력받은 값을 문자열로 변환시켜주기위해서 toString()메서드를 사용해줍니다.

- 그리고 BigInteger은 문자열이기에 사칙연산이 안됩니다. 그래서 BigInteger클래스의 내장함수를 사용하여 사칙연산을 해줘야합니다. 
- 사칙연산 내장함수는 아래의 표와 같습니다.

| 부호 | 함수 |
| ---  | -------- |
| 덧셈 | b1.add(b2) |
| 뺄셈 | b1.subtract(b2) |
| 곱셈 | b1.multiply(b2) |
| 나눗셈 | b1.divide(b2)|
| 나머지 | b1.remainder(b2) |