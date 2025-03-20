# 문제 1
```java
package lang.wrapper.test;  
  
public class WrapperTest1 {  
    public static void main(String[] args) {  
        String str1 = "10";  
        String str2 = "20";  
        int plus = Integer.parseInt(str1) + Integer.parseInt(str2);  
        System.out.println("두 수의 합 : " + plus);  
    }  
}
```

# 문제 2
```java
package lang.wrapper.test;  
  
public class WrapperTest2 {  
    public static void main(String[] args) {  
        String[] array = {"1.5", "2.5", "3.0"};  
        double sum = 0;  
        for(String s : array){  
        sum += Double.parseDouble(s);  
        }  
        System.out.println("sum = " + sum);  
    }  
}
```

# 문제 3
```java
package lang.wrapper.test;  
  
public class WrapperTest3 {  
    public static void main(String[] args) {  
        String str = "100";  
        Integer integer1 = Integer.valueOf(str);  
        int value = integer1.intValue();  
        Integer integer2 = Integer.valueOf(value);  
  
        System.out.println("integer1 = " + integer1);  
        System.out.println("value = " + value);  
        System.out.println("integer2 = " + integer2);  
    }  
}
```

# 문제 4
```java
package lang.wrapper.test;  
  
public class WrapperTest4 {  
    public static void main(String[] args) {  
        String str = "100";  
        Integer integer1 = Integer.valueOf(str);  
        int value = integer1;  
        Integer integer2 = value;  
  
        System.out.println("integer1 = " + integer1);  
        System.out.println("value = " + value);  
        System.out.println("integer2 = " + integer2);  
    }  
}
```

# 문제 5
```java
package lang.math.test;  
  
import java.util.Arrays;  
import java.util.Random;  
  
public class LottoGenerator {  
    public static void main(String[] args) {  
        int[] lottoNumbers = new int[6];  
        Random random = new Random();  
        for (int i = 0; i < lottoNumbers.length; i++) {  
            for (int j = 0; i < lottoNumbers.length; i++) {  
                lottoNumbers[i] = random.nextInt(1, 45);  
                if (i != j) {  
                    if (lottoNumbers[i] == lottoNumbers[j]) {  
                        System.out.println("중복!");  
                        lottoNumbers[i] = random.nextInt(1, 45);  
                    }  
                }  
            }  
        }  
        String lottoNumber = Arrays.toString(lottoNumbers);  
        System.out.println("로또 번호 = " + lottoNumber);  
    }  
}
```

# 문제 6
```java
package lang.math.test;  
  
import java.util.Arrays;  
import java.util.Random;  
  
public class LottoGenerator {  
    //로또 번호를 생성 및 검사 두부분으로 나눠야될듯  
    int[] lottoNumbers = new int[6];  
    int count = 0;  
    Random random = new Random();  
  
    public void lottoStart() {  
        System.out.println("로또 번호를 추첨하겠습니다.");  
        while (count < 6) {  
            int number = random.nextInt(1, 45);  
            if (numberCheck(number)) {  
                lottoNumbers[count] = number;  
                count++;  
            }  
        }  
        System.out.println("로또 번호 = " + Arrays.toString(lottoNumbers));  
    }  
  
    public boolean numberCheck(int number) {  
        for (int i = 0; i < lottoNumbers.length; i++) {  
            if (number == lottoNumbers[i])  
                return false;  
        }  
        return true;  
    }  
}
```

```java
package lang.math.test;  
  
public class LottoMain {  
    public static void main(String[] args) {  
        LottoGenerator lotto = new LottoGenerator();  
        lotto.lottoStart();  
    }  
}
```