```java
// if `m1()` already returns false then it does not execute `m2()` and `m3()`
boolean newBoolean = m1() && m2() && m3();

// if `m1()` already returns true then it does not execute `m2()` and `m3()`
boolean newBoolean = m1() || m2() || m3();

```
