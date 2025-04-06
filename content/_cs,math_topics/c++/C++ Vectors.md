- need to include `#include <vector>`
- more modern approach than [[C++ Arrays]]
### Basics
```c++
# syntax to create a vector
std::vector<type> name;
std::vector<float> calories;

# initializing a vector
std::vector<float> calories = {42.5, -943.5}

# initializing by presizing - setting the size
std::vector<float> calories(2);
# would look like {0.0, 0.0} since 0.0 is the default value for double

# indexing
std::vector<double> subway_child = {400, 600, 750};
std::cout << subway_child[2] << "\n";
```

### Operations
```c++
# adding elems to the 'back'
std::vector<double> subway_child = {400, 600};
subway_child.push_back(750); //{400,600,750}

# remove elements from the 'back'
subway_child.pop_back(); //{400, 600};
# no return value in c++

```

### Size
> [! Size of vector]
> `<std::vector>` not only stores the elements; it also stores the size of the vector:

![[Pasted image 20250121232049.png|400]]
- `.size()` function returns the number of elements in the vector

### Example
```c++
# first three multiples

#include <iostream>
#include <vector>

// Define first_three_multiples() here:
std::vector<int> first_three_multiples(int num)
{
  std::vector<int> res = {num * 1, num*2, num*3};
  return res;
}

int main() {
  for (int element : first_three_multiples(8)) {
    std::cout << element << "\n";
  }
}
```