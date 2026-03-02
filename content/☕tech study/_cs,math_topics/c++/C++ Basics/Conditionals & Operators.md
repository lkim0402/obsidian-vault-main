## Conditionals
- basically nothing to learn new

```c++
# ============ if cases
if (ph > 7)
{
std::cout << "Basic";
}
else if (ph < 7)
{
std::cout <<"Acidic";
}
else
{
std::cout << "Neutral";
}

# ============== switch cases

int main() {
    int num;

    std::cout << "Guess what I'm thinking: ";
    std::cin >> num; // no std::endl; for cin statements

    switch (num) {
        case 1:
            std::cout << "Correct!" << std::endl;
            break;
        default:
            std::cout << "Incorrect!" << std::endl;
            break;
    }

    return 0;
}
```

## Operators
- `&&, ||, !`
	- Keyword `and`, `or`,`not` can be used instead too!!!