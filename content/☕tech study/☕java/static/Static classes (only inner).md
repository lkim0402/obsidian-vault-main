```java
public class Outer {
    static class Inner {
        void show() {
            System.out.println("Inside static nested class");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer.Inner obj = new Outer.Inner();  // No need to create Outer instance
        obj.show();
    }
}

```