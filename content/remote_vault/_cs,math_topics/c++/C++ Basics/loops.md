### For loop
```c++
int main() {

  for (int i = 0; i < 10; i++) {
    std::cout << "I will not throw paper airplanes in class.\n";
  }
  
}
```

### While loop
```c++
#include <iostream>

int main() {

  int guess;
  
  int tries = 0;

  std::cout << "I have a number 1-10.\n";
  std::cout << "Please guess it: ";
  std::cin >> guess;
 
  while (guess != 8 && tries < 50)
  {
    std::cout << "Try again (" << 50 - tries << " remaining trials): ";
    std::cin >> guess;
    tries++;
  }
  
  if (guess == 8) {
    std::cout << "You got it!\n";
  }  
}
```