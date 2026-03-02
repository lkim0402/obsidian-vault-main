- dont use nested for loops omg
- first just track the smallest and biggest numbers, and just subtract them at the end

```java
public class GreatestDifferenceFinder {
    int greatestDifference(int[] intArray) {
        // 코드를 입력하세요.
        if (intArray.length < 2) {
            return 0;
        }
        
        int diff = 0;
        // for (int i = 0; i < intArray.length; i++)
        // {
        //     for (int j = i + 1; j < intArray.length; j++)
        //     {
        //         diff = Math.max(Math.abs(intArray[i] - intArray[j]), diff);
        //         // System.out.println("abs(intArray[" + i 
        //         //     + "] - intArray[" + j + "]) = " 
        //         //     + diff);
        //     }
        // }
        
        //진짜 대박...
        // find the biggest/smallest value, then subtract it
        int min = intArray[0];
        int max = intArray[0];
        
        // 2 6 4
        for (int i = 0; i < intArray.length; i++)
        {
            int cur = intArray[i];
            if (cur < min)
            {
                min = cur;
            }
            if (cur > max)
            {
                max = cur;
            }
        }
        return max - min;
    }
}
```