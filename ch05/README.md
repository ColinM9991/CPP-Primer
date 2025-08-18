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

## Exercise 5.4

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

# Exercise Section 5.3.1

## Exercise 5.5

### Question:

Using an `if-else` statement, write your own version of the program to generate the letter grade from a numeric grade;

### Answer:

```cpp
#include <iostream>
#include <vector>

int main() {
    const std::vector<std::string> scores{"F", "D", "C", "B", "A", "A++"};

    unsigned grade;
    if (std::cin >> grade && grade <= 100) {
        const std::string *letterGrade;
        if (grade < 60) {
            letterGrade = &scores[0];
        } else {
            letterGrade = &scores[(grade - 50) / 10];
        }

        std::cout << "Grade: " << *letterGrade << std::endl;
    } else {
        std::cout << "Unspported grade. Grade must be between 0-100" << std::endl;
    }
}
```

## Exercise 5.6

### Question:

Rewrite your grading program to use the conditional operator in place of the `if-else` statement.

### Answer:

```cpp
#include <iostream>
#include <vector>

int main() {
    const std::vector<std::string> scores{"F", "D", "C", "B", "A", "A++"};

    unsigned grade;
    if (std::cin >> grade && grade <= 100) {
        const auto index = grade >= 60 ? (grade - 50) / 10 : 0;
        const std::string *letterGrade = &scores[index];

        std::cout << "Grade: " << *letterGrade << std::endl;
    } else {
        std::cout << "Unspported grade. Grade must be between 0-100" << std::endl;
    }
}
```

## Exercise 5.7

### Question:

Correct the errors in each of the following code fragments

```cpp
(a) 
if (ival1 != ival2)
    ival1 = ival2
else ival1 = ival2 = 0;
(b)
if (ival < minval)
    minval = ival;
    occurs = 1;
(c)
if (int ival = get_value())
    cout << "ival = " << ival << endl;
if (!ival)
    cout << "ival = 0\n";
(d) 
if (ival = 0)
    ival = get_value();
```

### Answers:

(A)
```cpp
if (ival1 != ival2)
    ival1 = ival2;
else ival1 = ival2 = 0;
```

(B)
```cpp
if (ival < minval)
{
    minval = ival;
    occurs = 1;
}
```

(C)
```cpp
if (int ival = get_value())
    std::cout << "ival = " << ival << std::endl;
else if (!ival)
    std::cout << "ival = 0" << std::endl;
```

(D)
```cpp
if (ival == 0)
    ival = get_value();
```

## Exercise 5.8

### Question:

What is a "dangling `else`"? How are `else` clauses resolved in C++?

### Answer:

A "dangling `else`" defines the ambiguity to which `if` statement an `else` branch belongs to.

In C++, an `else` clause is resolved in relation to its nearest preceding `if` statement that is defined within the same block.

An `else` statement can be paired with an `if` statement by adding braces around the nested statement.