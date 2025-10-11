# Exercise Section 6.1

## Exercise 6.1

### Question

What is the difference between a parameter and an argument?

### Answer

A parameter defines the values which a function expects when being called. An argument is the initializer for the function parameter when being called.

## Exerise 6.2

### Question

Indicate which part of the following functions are in error and why. Suggest how you might correct the problems;

a.
```cpp
int f() {
    std::string s;
    // ...
    return s;
}
```

b.
```cpp
f2(int i) { /* */ }
```

c.
```cpp
int calc(int v1, int v1) /* ... */ }
```

d.
```cpp
double square(double x) return x * x;
```

### Answers

a.
```cpp
std::string f() {
    std::string s;
    // ...
    return s;
}
```

b.
```cpp
void f2(int i) { /* */ }
```

c.
```cpp
int calc(int v1, int v2) { /* ... */ }
```

d.
```cpp
double square(double x) { return x * x; }
```

## Exercise 6.3

### Question

Write and test your own version of fact

### Answer

```cpp
int fact(int val) {
    int ret = 1;

    while (val > 1)
        ret *= val--;

    return ret;
}

int main() {
    const int result = fact(5);

    if (result == 120)
        std::cout << "Result is 120" << std::endl;
    else
        std::cout << "Result is not 120" << std::endl;

    return EXIT_SUCCESS;
}
```

## Exercise 6.4

### Question

Write a function that interacts with the user, asking for a number and generating the factorial of that number. Call this function from main.

### Answer

```cpp
int fact() {
    int val = 0;

    std::cout << "Enter a number: ";

    if (!(std::cin >> val)) {
        std::cout << "Invalid number" << std::endl;
        return 0;
    }

    int ret = 1;

    while (val > 1)
        ret *= val--;

    return ret;
}

int main() {
    const int result = fact();

    std::cout << "Factorial is " << result << std::endl;

    return EXIT_SUCCESS;
}
```

## Exercise 6.5

### Question

Write a function to retunr the absolute value of its arugment

### Answer

```cpp
int abs(int val) {
    return val < 0 ? -val : val;
}

int main() {
    const int result = abs(-50);

    std::cout << "Absolute is " << result << std::endl;

    return EXIT_SUCCESS;
}
```

# Exercise Section 6.1.1

## Exercise 6.6

### Question

Explain the differences between a parameter, a local variable, and a local static variable. Give an example of a function in which each might be useful.

### Answers

Parameters are defined within the scope of a function definition. All parameters are local variables. However, additional variables defined within the function body are also local variables.

A local variable is one that is defined within a scope and it becomes undefined once the scope has been exited.

A local static variable persists throughout the life of the application and is initialized before control flows through the function for the first time. The value is retained.

Local static:
```cpp
size_t count_calls() {
    static size_t counter = 0; // local static
    return ++counter;
}
```

Parameter and local variable
```cpp
int abs(int val) // parameter 
{
    int local_val = val // local variable
    
    return local_val < 0 ? -local_val : local_val;
}
```

## Exercise 6.7

### Question

Write a function that returns 0 when it is first called and then generates numbers in sequence each time it is called again.

### Answer

```cpp
size_t counter() {
    static size_t counter = 0;

    return counter++;
}
```

# Exercise Section 6.1.2

## Exercise 6.8

### Question

Write a header file named `Chapter6.h` that contains declarations for the functions you wrote for the exercises in § 6.1

### Answer

```cpp
#ifndef CHAPTER6_H
#define CHAPTER6_H

int f();
void f2(int i);
int calc(int v1, int v2);
double square(double x);
int fact(int val);
int abs(int val);

#endif
```

# Exercise Section 6.1.3

## Exercise 6.9

### Question

Write your own versions of the `fact.cc` and `factMain.cc` files. These files should include your `Chapter6.h` from the exercises in the previous section. Use these files to understand how your compiler supports separate compilation.

### Answer

fact.cpp
```cpp
#include "Chapter6.h"

int fact(int val) {
    int ret = 1;

    while (val > 0)
        ret *= val--;

    return ret;
}
```

factMain.cpp
```cpp
#include <iostream>
#include <ostream>

#include "Chapter6.h"

int main() {
    const auto val = fact(5);

    std::cout << val << std::endl;

    return EXIT_SUCCESS;
}
```

```
clang++ -c factMain.cpp
clang++ -c fact.cpp
clang++ factMain.o fact.o -o main
```

# Exercise Section 6.2.1

## Exercise 6.10

### Question

Using pointers, write a function to swap the values of two `ints`. Test the function by calling it and printing the swapped values.

### Answer

```cpp
void swap(int *a, int *b) {
    const int temp = *a;

    *a = *b;
    *b = temp;
}

int main() {
    int a = 5;
    int b = 10;

    std::cout << "Before " << a << " " << b << std::endl;

    swap(&a, &b);

    std::cout << "After  " << a << " " << b << std::endl;

    return EXIT_SUCCESS;
}
```

# Exercise Section 6.2.2

## Exerise 6.11

### Question

Write and test your own version of `reset` that takes a reference

### Answer

```cpp
void reset(int &i) {
    i = 0;
}

int main() {
    int j = 5;

    std::cout << j << std::endl; // 5

    reset(j);

    std::cout << j << std::endl; // 0

    return EXIT_SUCCESS;
}
```

## Exercise 6.12

### Question

Rewrite the program from exercise 6.10 in § 6.2.1 to use references instead of pointers to swap the value of two `ints`. Which version do you think would be easier to use and why?

### Answer

Using references is easier to use as there's no consideration required in terms of using the address-of operator or dereferencing the variables `a` and `b` through normal pointer operation.

```cpp
void swap(int &a, int &b) {
    const int temp = a;

    a = b;
    b = temp;
}

int main() {
    int a = 5;
    int b = 10;

    std::cout << "Before " << a << " " << b << std::endl;

    swap(a, b);

    std::cout << "After  " << a << " " << b << std::endl;

    return EXIT_SUCCESS;
}
```

## Exercise 6.13

### Question

Assuming `T` is the name of a type, explain the difference between a function declared as `void f(T)` and `void f(T&)`

### Answer

The former function declaration is called by value, meaning the value of the argument is copied. This can be expensive if it's a value of an indeterminate size.

In the latter instance, the argument is passed by reference. No copies are takne and it follows the standard conventions of a reference.

## Exercise 6.14

### Question

Give an example of when a parameter should be a reference type. Give an example of when a parameter should not be a reference.

### Answer

A parameter should be a reference type when it is either a non-copyable type (IO types) or a container type.

```cpp
void func(std::string &val);
```

On the other hand, parameters of a known-size that are small can be passed by value since the performance impact is negligible

```cpp
void func(int val);
```

## Exercise 6.15

### Question

Explain the rationale for the type of each of `find_char`'s parameters. In particular, why is `s` a reference to `const` but `occurs` is a plain reference? Why are these parameters references, but the `char` parameter `c` is not? What would happen if we made `s` a plain reference? What if we made `occurs` a reference to const?

### Answer

#### Why is `s` a reference to `const` but `occurs` is a plain reference?

`s` is a reference to `const` because the object to which `s` is bound to is not changed by the scope of the function. `occurs` is a plain reference because the object to which `occurs` binds to is updated.

#### Why are these parameters references, but the `char` parameter `c` is not?

`char` consumes 8-bits of memory. It is passed by value because it is a very small type. 

`s` is a reference to `const` as copying it may be an expensive operation

`occurs` is a reference since the object to which it is bound is updated for the caller to track the number of times a character occurs in a `string`

#### What would happen if we made `s` a plain reference?

It would allow the `find_chars` function to change the contents of `s`, thus changing the value of the object to which `s` is bound to

#### What if we made `occurs` a reference to `const`?

This would produce a compilation error at the point of trying to increment `occurs` within the loop. `consts` variables and references to `const` cannot be changed.

# Exercise Section 6.2.3

## Exercise 6.16

### Question

The following function, although legal, is less useful than it might be. Identify and correct the limitation on this function

```cpp
bool is_empty(std::string &s) { return s.empty(); }
```

### Answer

The function uses a reference to `std::string` as a parameter. This limits a caller to working with non-`const` objects and may also give the impression of this function changing the value in `s`.

The solution is to use a reference to `const`

```cpp
bool is_empty(const std::string &s) { return s.empty(); }
```

## Exercise 6.17

### Question

Write a function to determine whether a `string` contains any capital letters. Write a function to change a `string` to all lowercase. Do the parameters you used in these functions have the same type? If so, why? If not, why not?

### Answer

Both functions use different parameter types. The first is a reference to `const` because it doesn't require access to change the `string` since it is simply checking whether any uppwercase letters exist.

The second function converts any uppercase characters in the `string` to lowercase. As such, it is a reference to `string` so that it is able to change the `string`.

```cpp
bool contains_upper(const std::string &s) {
    return std::any_of(s.cbegin(), s.cend(), isupper);
}

void tolower(std::string &s) {
    for (auto &c : s) {
        if (std::isupper(c))
            c = std::tolower(c, std::locale());
    }
}
```

## Exercise 6.18

### Question

Write declarations for each of the following functions. When you write these declarations, use the name of the function to indicate what the function does.

(a) A function named `compare` that returns a bool and has two parameters that are references to a class named `matrix`

(b) A function named `change_val` that returns a `vector<int>` iterator and takes two parameters. One is an `int` and the other is an iterator for a `vector<int>`

### Answer

a.

```cpp
bool compare(matrix &x, matrix &y);
```

b.
```cpp
vector<int>::iterator change_val(int a, std::vector<int>::iterator iter);
```

## Exercise 6.19

### Question

Given the following declarations, determine which calls are legal and which are illegal. For those that are illegal, explain why.

```cpp
double calc(double);
int count(const string &, char);
int sum(std::vector<int>::iterator, std::vector<int>::iterator, int);
vector<int> vec(10);
```

a. `calc(23.4, 55.1);`  
b. `count("abcda", 'a');`  
c. `calc(66);`  
d: `sum(vec.begin(), vec.end(), 3.8);`

### Answers

a. Illegal - too many arguments  
b. Legal  
c. Legal  
d. Legal - `double` converted to `int`

## Exercise 6.20

### Question

When should reference parameters be reference sto `const`? What happens if we make a parameter a plain reference when it could be a reference to `const`?

### Answer

A parameter should be a reference to `const` when it makes no changes to the value to which the parameter has bound. This ensures maximum API compatibility.

Using plain references prevents callers from passing references to `const` further up the call stack and may result in suboptimal solutions such as changing parameter types of functions in the call stack.

# Exercise Section 6.2.4

## Exercise 6.21

### Question

Write a function that takes an `int` and a pointer to an `int` and returns the larger of the `int` value or the value to which the pointer points. Which type should you use for the pointer?

### Answer

```cpp
int get_largest(int a, const int *b) {
    return a > *b ? a : *b;
}
```

## Exercise 6.22

### Question

Write a function to swap two `int` pointers

### Answer

```cpp
void swap(int *&x, int *&y) {
    std::swap(x, y);
}
```

## Exercise 6.23

### Question

Write your own versions of each of the `print` functions presented in this section. Call each of these functions to print `i` and `j` defined as follows

```cpp
int i = 0, j[2] = {0, 1};
```

### Answer

```cpp
void print(const int *ip) {
    if (ip) {
        std::cout << *ip << std::endl;
    }
}

void print(const int *beg, const int *end) {
    while (beg != end)
        std::cout << *beg++ << std::endl;
}

void print(const int ia[], std::size_t size) {
    for (std::size_t i = 0; i < size; ++i) {
        std::cout << ia[i] << std::endl;
    }
}

void print(const int (&arr)[2]) {
    for (const auto &i: arr) {
        std::cout << i << std::endl;
    }
}

int main() {
    int i = 0, j[2] = {0, 1};
    print(&i);
    print(std::begin(j), std::end(j));
    print(j, std::end(j) - std::begin(j));
    print(j);

    return EXIT_SUCCESS;
}
```

## Exercise 6.24

### Question

Explain the behavior of the following function. If there are problems in the code, explain what they are and how you might fix them

```cpp
void print(const int ia[10]) {
    for (size_t i = 0; i != 10; ++i)
        std::cout << ia[i] << std::endl;
}
```

### Answer

The array size, as part of the parameter list, is meaningless in this instance since `const int ia[10]` is implicitly a `const int *` to the first element of the array. To impose a size constraint on the array used as an argument, it can be passsed by reference

```cpp
void print(const int (&ia)[10]) {
    for (int i : ia)
        std::cout << i << std::endl;
}
```

Alternatively, a size can be passed in
```cpp
void print(const int *ia, const std::size_t size) {
    for (size_t i = 0; i != size; ++i)
        std::cout << ia[i] << std::endl;
}
```

Or pointers to the first and last elements
```cpp
void print(const int *beg, const int *end) {
    while (beg != end)
        std::cout << *beg++ << std::endl;
}
```

# Exercise Section 6.2.5

## Exercise 6.25

### Question

Write a `main` function that takes two arguments. Concatenate the supplied arguments and print the resulting `string`

### Answer

```cpp
int main(int argc, char *argv[]) {
    if (argc < 3) {
        std::cerr << "Insufficient arguments" << std::endl;
        return EXIT_FAILURE;
    }

    std::cout << argv[1] << " " << argv[2] << std::endl;

    return EXIT_SUCCESS;
}
```

## Exercise 6.16

### Question

Write a program that accepts the options presented in this section. Print the values of the arguments passed to `main`

### Answer

```cpp
int main(int argc, char *argv[]) {
    if (argc == 1) {
        std::cerr << "Insufficient arguments" << std::endl;
        return EXIT_FAILURE;
    }

    for (int i = 1; i < argc; i++) {
        std::cout << argv[i] << std::endl;
    }

    return EXIT_SUCCESS;
}
```

# Exervise Section 6.2.6

## Exercise 6.27

### Question

Write a function that takes an `initializer_list<int>` and produces the sum of hte elements in the list.

### Answer

```cpp
int sum(std::initializer_list<int> items) {
    int total = 0;

    for (const auto item : items)
        total += item;

    return total;
}
```

## Exercise 6.28

### Question

In the second version of `error_msg` that has an `ErrCode` parameter, what is the type of `elem` in the `for` loop?

### Answer

`elem` is of type `const std::string&` (`std::string` reference to `const`)

## Exercise 6.29

### Question

When you use an `initializer_list` in a range `for` would you ever use a reference as the loop control variable? If so, why? If not, why not?

### Answer

Same rules as exercise § 6.13 apply. If it is a large type then using a reference is more performance friendly. Whereas if the type is small, such as an integer, then passying by value is fine.

# Exercise Section 6.3.2

## Exercise 6.30

### Question

Compile the version of `str_subrange` as presented on page 223 to see what your compiler does with the indicated errors.

### Answer

```cpp
bool str_subrange(const std::string &str1, const std::string &str2) {
    if (str1.size() == str2.size())
        return str1 == str2;

    auto size = (str1.size() < str2.size()) ? str1.size() : str2.size();

    for (decltype(size) i = 0; i != size; ++i) {
        if (str1[i] != str2[i])
            return;
    }
}
```

> error: non-void function 'str_subrange' should return a value [-Wreturn-mismatch]

## Exercise 6.31

### Question

When is it valid to return a reference? A reference to `const`?

### Exercise

It is valid to return a reference, or a reference to `const`, to a preexisting object whose lifetime outlives that of the function.

References and pointers should not be returned to objects whose lifetime is local to the function scope.

References to preexisting values that are returned from functions are lvalues and can be used where an lvalue is required, such as assignment.

## Exercise 6.32

### Question

Indicate whether the following function is legal. If so, explain what it does; if not, correct any errors and then explain it.

```cpp
int &get(int *arry, int index) { return arry[index]; }
int main() {
    int ia[10];
    for (int i = 0; i != 10; ++i)
        get(ia, i) = i;
}
```

### Answer

This function definition is legal. It returns a reference to an array index for a preexisting object. Since references returned from functions are lvalues, the `for`-loop assigns the value of `i` to the index `i`.

The resulting values of the array will be `0, 1, 2, 3, 4, 5, 6, 7, 8, 9`

## Eercise 6.33

### Question

Write a recursive function to print the contents of a `std::vector`

### Answer

```cpp
void recurse(std::vector<int>::iterator beg, std::vector<int>::iterator end) {
    if (beg == end) return;

    std::cout << *beg << std::endl;

    recurse(++beg, end);
}

int main() {
    std::vector<int> v{0, 1, 2, 3, 4, 5};

    recurse(v.begin(), v.end());
}
```

## Exercise 6.34

### Question

What would happen if the stopping condition in `factorial` were `if (val != 0)

```cpp
int factorial(int val) {
    if (val > 1)
        return factorial(val-1) * val;

    return 1;
}
```

### Answer

With the current input the function returns as expected. However, if we pass a negative integer then the function will run until the call stack overflows

## Exercise 6.35

### Question

In the call to `factorial`, why did we pass `val - 1` rather than `val--`

## Exercise

`val--` would also result in an recursion loop since it returns the original value.

`--val` would result in incorrect multiplication as it would change `val` within that call stack.

`val - 1` correctly decrements the value for the next recursive call.

# Exercise Section 6.3.3

## Exercise 6.36

### Question

Write the declaration for a function that returns a reference to an array of ten `strings`, without using either a trailing return, `decltype`, or a type alias.

### Answer

```cpp
std::string (&get_strings())[10];
```

## Exercise 6.37

### Question

Write three additional declarations for the function in the previous exercise. One should use a type alias, one should use a trailing return, and the third should use `decltype`. Which form do you prefer and why?

### Answer

```cpp
std::string STRINGS[10] = { ... };
typedef std::string stringT[10];

stringT &get_strings();
auto get_strings() -> std::string(&)[10]
decltype(STRINGS) &get_strings();
```

All 3 options are perfectly readable and produce the same result so I have no preference.

## Exercise 6.38

### Question

Revise the `arrPtr` function to return a reference to the array

```cpp
int odd[] = {1, 3, 5, 7, 9};
int even[] = {0, 2, 4, 6, 8};

decltype(odd) *arrPtr(int i) {
    return (i % 2) ? &odd : &even;
}
```

### Answer

```cpp
int odd[] = {1, 3, 5, 7, 9};
int even[] = {0, 2, 4, 6, 8};

decltype(odd) &arrPtr(int i) {
    return (i % 2) ? odd : even;
}
```

# Exercise Section 6.4

## Exercise 6.39

### Question

Explain the effect of the second declaration in each one of the following sets of declarations. Indicate which, if any, are illegal

a.
```cpp
int calc(int, int);
int calc(const int, const int);
```

b.
```cpp
int get();
double get();
```

c.
```cpp
int *reset(int *);
double *reset(double *);
```

### Answer

a. Legal - the second declaration redeclares the same function because parameters with top-level consts are indistinguishable from those without

b. Illegal - the return type cannot be the only difference across two overloaded functions

c. Legal - the parameter types are different. Which function is picked by the compiler depends on the arguments

# Exercise Section 6.5.1

## Exercise 6.40

### Question

Which, if either, of the following declarations are errors? Why?

a.
```cpp
int ff(int a, int b = 0, int c = 0);
```

b.
```cpp
char *init(int ht = 24, int wd, char bckgrnd);
```

### Answers

a. Legal - default argument declarations specified for all parameters

b. Illegal - Defaults must be specified in a right to left order

## Exercise 6.41

### Question

Which, if any, of the following calls are illegal? Why? Which, if any, are legal but unlikely to match the programmer's intent? Why?

```cpp
char *init(int ht, int wd = 80, char bckgrnd = ' ');
```

a. `init()`  
b. `init(24, 10);`  
c. `init(14, '*');`

### Answer

a. Illegal - argument not specified for `ht`
b. Legal
c. Legal but incorrect behavior. `'*'` is a `char` that is convertible to `int`. * = 0x2A = 42

## Exercise 6.42

### Question

Give the third parameter of `make_plural` (§ 6.3.2) a default argument of 's'. Test your program by printing singular and plural versions of the words `success` and `failure`

### Answer

```cpp
std::string make_plural(size_t ctr,
                        const std::string &word,
                        const std::string &ending = "s") {
    return (ctr > 1) ? word + ending : word;
}

int main() {
    std::cout << make_plural(1, "success") << std::endl;
    std::cout << make_plural(2, "failure") << std::endl;
}
```

# Exercise Section 6.5.2

## Exercise 6.43

### Question

Which one of the following declarations and definitions would you put in a header? In a source file? Explain why.

a.
```cpp
inline bool eq(const BigInt&, const BigInt&) { ... }
```

b.
```cpp
void putValues(int *arr, int size);
```

### Answer

a. This could be defined in both a header and source file but it should ideally be done so in a header

b. This is a declaration and should be declared in a header

## Exercise 6.44

### Question

Rewrite the `isShorter` function from § 6.2.2 to be `inline`

### Answer

```cpp
inline bool isShorter(const std::string &s1, const std::string &s2) {
    return s1.size() < s2.size();
}
```

## Exercise 6.45

### Question

Review the programs you've written for the earlier exercises and decide whether they should be defined as `inline`. If so, do so. If not, explain why they should not be `inline.

### Answer

The book is not explicit enough and doesn't make clear whether it should be every previous exercise, or the exercises within this chapter.

The generic answer is: any functions that can be inlined should be inlined.

## Exercise 6.46

### Question

Would it be possible to define `isShorter` as a `constexpr`? If so, do so. If not, explain why not.

### Answer

`isShorter` cannot be marked as `constexpr` because it  uses `.size()` of `std::string` which is not a literal or constant expression

# Exercise Section 6.5.3

## Exercise 6.47

### Question

Revise the program you wrote in the exercises in § 6.3.2 that used recursion to print the contents of a `std::vector` to conditionally print information about its execution. For example, you might print the size of the `std::vector` on each call. Compile and run the program with debugging turned on and again with it turned off.

### Answer

```cpp
void recurse(std::vector<int>::iterator beg, std::vector<int>::iterator end) {
    if (beg == end) return;

#ifndef NDEBUG
    const auto size = std::distance(beg, end);

    std::cout
            << __func__ << std::endl
            << "Remaining size is: " << size << std::endl
            << __FILE__ << std::endl;
#endif
    std::cout << *beg << std::endl;

    recurse(++beg, end);
}
```

## Exercise 6.48

### Question

Explain what this loop does and whether it is a good use of `assert`

```cpp
std::string s;
while (std::cin >> s && s != sought) ;
assert(cin);
```

### Answer

The loop reads input from `std::cin` until the input matches a sought-after word.

The usage of `assert` is not a good use because it may capture user input from `std::cin`. The assertion expression should instead ensure that `s` is equal to `sought` since this is a scenario that should never occur under normal execution.

```cpp
assert(s == sought);
```

# Exercise Section 6.6

## Exercise 6.49

### Question

What is a candidate function? What is a viable function?

### Answer

A candidate function is one whose name matches the function call and is visible at the point of being called.

A viable function is one whose number of parameters match the arguments of the function call and where the types are either a direct match or convertible to the parameter type.

## Exercise 6.50

### Question

Given the declarations for `f` from page 242, list the viable functions, if any for each of the following calls. Indicate which function is the best match, or if the call is illegal whether there is no match or why the call is ambiguous.

```cpp
void f();
void f(int);
void f(int, int);
void f(double, double = 3.14);

//a
f(2.56, 42)
// b
f(42)
// c
f(42, 0)
// d
f(2.56, 3.14)
```

### Answers

a.
- Viable functions
  - `f(int, int)`
  - `f(double, double = 3.14)`
- Best match: ambiguous

b.  
- Viable functions
  - `f(int)`
  - f(double, double = 3.14)`
- Best match: `f(int)`

c.
- Viable functions
  - f(int, int)
  - f(double, double = 3.14)
- Best match: `f(int, int)`

d.
- Viable functions
  - f(int, int)
  - f(double, double = 3.14)
- Best match: `f(double, double = 3.14)`

## Exercise 6.51

### Question

Write all four versions of `f`. Each function should print a distinguishing message. Check your answers for the previous exercise. If your answers were incorrect, study this section until you understand why your answers were wrong.

### Answer

See previous exercise answers

# Exerise Section 6.6.1

## Exercise 6.52

### Question

Given the following declarations

```cpp
void manip(int, int);
double dobj;
```

what is the rank of each conversion in the following calls?

a. `manip('a', 'z');`
b. `manip(55.4, dobj);`

### Answer

a. Promotion - `char` promoted to `int`
b. Conversion - arthmetic conversion

## Exercise 6.53

### Questions

Explain the effect of the second declaration in each one of the following sets of declarations. Indicate which, if any, are illegal

```cpp
// a
int calc(int&, int&);
int calc(const int&, const int&);

// b
int calc(char*, char*);
int calc(const char*, const char*);

// c
int calc(char*, char*);
int calc(char* const, char* const);
```

### Answer

```cpp
// a - valid
int calc(int&, int&); // reference to int
int calc(const int&, const int&); // const reference to int

// b - valid
int calc(char*, char*); // pointer to char
int calc(const char*, const char*); // const pointer to char

// c - invalid, ambiguous functions as low-level const is not part of distinct ranking
int calc(char*, char*);
int calc(char* const, char* const);
```

# Exercise Section 6.7

## Exercise 6.54

### Question

Write a declaration for a function that takes two `int` parameters and returns an `int`, and declare a `vector` whose elements have this function pointer type.

### Answer

```cpp
int calc(int x, int y);

int main() {
    std::vector<int (*)(int, int)> v;
}
```

## Exercise 6.55

### Question

Write four functions that add, subtract, multiply and divide two `int` values. Store pointers to these functions in your `vector` from the previous exercise.

### Answer

```cpp
 
int calc(int x, int y);

int add(int x, int y) { return x + y; }
int subtract(int x, int y) { return x - y; }
int multiply(int x, int y) { return x * y; }
int divide(int x, int y) { return x / y; }

int main() {
    std::vector<int (*)(int, int)> v{add, subtract, multiply, divide};
}
```

## Exercise 6.56

### Quesiton

Call each element in the `vector` and print their result

### Answer

```cpp
int calc(int x, int y);

int add(int x, int y) { return x + y; }
int subtract(int x, int y) { return x - y; }
int multiply(int x, int y) { return x * y; }
int divide(int x, int y) { return x / y; }

int main() {
    std::vector<int (*)(int, int)> v{add, subtract, multiply, divide};

    int x = 5, y = 2;
    for (int (*f)(int, int): v) {
        const int result = f(x, y);

        std::cout << result << std::endl;
    }
}
```