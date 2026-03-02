```java
import java.util.Scanner;  
  
public class HelloWorld {  
    public static void main(String[] args){  
        Scanner in = new Scanner(System.in);  
        System.out.println("What's your name? ");  
        String name = in.nextLine();  
        System.out.println("What's your age? ");  
        int age = in.nextInt();  
  
        System.out.println("Hello " + name + ", " + age + " years old.");  
    }  
}
```

```
What's your name? 
Leejun
What's your age? 
23
Hello Leejun, 23 years old.
```