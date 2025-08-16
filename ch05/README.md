# Exercise Section 5.1

## Exercise 5.1

### Question:

What is a null statement? When might you use a null statement?

### Answer:

A null statement, also known as an empty statement, is a statement consisting of a single semi-colon (`;`). 

The null statement can be used when the language expects a statement but the program logic has no such need.  
For instance, if the condition block of a while loop can succinctly handle the program logic, a null statement can be used to supply an empty statement thus satisfying the language requirements

Example:
```cpp
#include <iostream>

int main() {
    constexpr int sought = 10;

    int i;
    while (std::cin >> i && i != sought)
        ; // Null statement

    std::cout << i << " " << sought << std::endl;

    return 0;
}
```

## Exercise 5.2

### Question:

What is a block? When might you use a block?

### Answer:

A block (or compound statement) is a series of statements that are wrapped in curly braces. The presence of curly braces creates a scope with which variables can be defined.  

Variables defined within a block (and thus a scope) follow the same scoping rules outlined on page 48. Variables defined within the scope are accessible from that scope and any nested scopes.  

A block is typically used when when an application requires multiple statements where a single statement would otherwise by expected.

Example:
```cpp
#include <iostream>

int main() {
    const int isDivisible = std::rand() % 100 == 0;

    if (isDivisible)
    { // Block begin
        std::cout << "Number was divisible by 100" << std::endl;
    } // Block end
    
    return 0;
}
```

## Exercise 5.3

### Question:

Use the comma operator to rewrite the `while` loop from page 11 so that it no longer requires a block. Explain whether this rewrite improves or diminishes the readability of this code

### Answer:

#### 1 - Comma operator in condition
```cpp
#include <iostream>

int main() {
    int sum = 0, val = 1;

    while (sum += val, val++, val <= 10)
        ;

    std::cout << "Sum of 1 to 10 inclusive is " << sum << std::endl;

    return 0;
}
```

#### 2 - Comma operator in statement

```cpp
#include <iostream>

int main() {
    int sum = 0, val = 1;

    while (val <= 10)
        sum += val, ++val;

    std::cout << "Sum of 1 to 10 inclusive is " << sum << std::endl;

    return 0;
}
```

Both versions affect the readability of the application by compressing the logic into a single line.  

In the case of the first version there is a bigger risk since the result of a comma expression is the result or value of the right-hand expression. If `val <= 10` switches places with `val++` or `sum += val` then the behaviour is undefined.


# Exercise Section 5.2

## Exercise 5.2

### Question:

Explain each of the following examples, and correct any problems you detect

### Problem 1:
```cpp
while (string::iterator iter != s.end()) { }
```

### Answer:

The solution is to declare and initialize the iterator outside of the control scope. The iterator should be incremented to ensure the loop finishes.

```cpp
auto iter = s.begin();
while (iter != s.end()) { ++iter; }
```

### Problem 2:
```cpp
while (bool status = find(word)) { }
if (!status) { }
```

### Answer:
There are two possible solutions.

1. declare the variable outside of the control structure.
```cpp
bool status;
while (status = find(word)) { }
if (!status) { }
```

2. move the condition to inside the loop
```cpp
while (bool status = find(word)) {
    if (!status) { }
}
```

