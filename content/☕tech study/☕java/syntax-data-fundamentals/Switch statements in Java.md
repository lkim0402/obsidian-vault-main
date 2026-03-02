
- not boolean but using the values itself
```java
int x = getInt();
switch(x)
{
	case 1:
		System.out.println("1");
		break;
	case 2:
		System.out.println("2");
		break;
	default:
		System.out.println("hello");
}
```

```java
int i = 10;
if (i % 3 == 0) { // i < 20 : 불린 식, 변수, 메소드
    System.out.println("C 구역입니다.");
} else if (1 % 3 == 1) {
    System.out.println("A 구역입니다.");
} else {
    System.out.println("B 구역입니다.");
}

// this is easier to use
switch (i % 3) { // i : 불린이 아닌 식, 변수, 메소드
    case 0:
        System.out.println("C 구역입니다.");
        break;
    case 1:
        System.out.println("A 구역입니다.");
        break;
    default:
        System.out.println("B 구역입니다.");
        break;
}
// u can use it to let it "fall"
switch (grade) {
    case "A+":
    case "A":
    case "B":
        // kinda like grouping A+,A,B
        System.out.println("참 잘했어요!"); 
        break;
    case "C":
    case "D":
        System.out.println("조금만 더 노력해 볼까요?");
        break;
    case "F":
        System.out.println("Fail입니다.");
    default:
        System.out.println("다시 수강해주세요.");
        break;
}


```