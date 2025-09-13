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

# Execise Section 5.3.2

## Exercise 5.9

### Question:

Write a program using a series of `if` statements to count the number of vowels in text read from `cin`.

### Answer:

```cpp
#include <iostream>

using namespace std;

int main() {
    string input;

    getline(cin, input);

    unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0;
    for (const auto c: input) {
        if (c == 'a')
            aCnt++;
        else if (c == 'e')
            eCnt++;
        else if (c == 'i')
            iCnt++;
        else if (c == 'o')
            oCnt++;
        else if (c == 'u')
            uCnt++;
    }

    const auto total = aCnt + eCnt + iCnt + oCnt + uCnt;
    cout << "Count of a: " << aCnt << endl
            << "Count of e: " << eCnt << endl
            << "Count of i: " << iCnt << endl
            << "Count of o: " << oCnt << endl
            << "Count of u: " << uCnt << endl
            << "Total vowels: " << total << endl;
}
```

## Exercise 5.10

### Question:

There is one problem with our vowel-counting program as we've implemented it:

It doesn't count capital letters as vowels. Write a program that counts both lower and uppercase letters as the appropriate vowel - that is, your program should count both 'a'and 'A' as part of `aCnt`, and so forth.

### Answers:

1. Logical OR operators

```cpp
#include <iostream>

using namespace std;

int main() {
    string input;

    getline(cin, input);

    unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0;
    for (const auto c: input) {
        if (c == 'a' || c == 'A')
            aCnt++;
        else if (c == 'e' || c == 'E')
            eCnt++;
        else if (c == 'i' || c == 'I')
            iCnt++;
        else if (c == 'o' || c == 'O')
            oCnt++;
        else if (c == 'u' || c == 'U')
            uCnt++;
    }

    const auto total = aCnt + eCnt + iCnt + oCnt + uCnt;
    cout << "Count of a: " << aCnt << endl
            << "Count of e: " << eCnt << endl
            << "Count of i: " << iCnt << endl
            << "Count of o: " << oCnt << endl
            << "Count of u: " << uCnt << endl
            << "Total vowels: " << total << endl;
}
```

2. Lowercasing

```cpp
#include <iostream>

using namespace std;

int main() {
    string input;

    getline(cin, input);

    unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0;
    for (const auto c: input) {
        auto lowered = tolower(c);
        if (lowered == 'a')
            aCnt++;
        else if (lowered == 'e')
            eCnt++;
        else if (lowered == 'i')
            iCnt++;
        else if (lowered == 'o')
            oCnt++;
        else if (lowered == 'u')
            uCnt++;
    }

    const auto total = aCnt + eCnt + iCnt + oCnt + uCnt;
    cout << "Count of a: " << aCnt << endl
            << "Count of e: " << eCnt << endl
            << "Count of i: " << iCnt << endl
            << "Count of o: " << oCnt << endl
            << "Count of u: " << uCnt << endl
            << "Total vowels: " << total << endl;
}
```

3. Switch

```cpp
#include <iostream>

using namespace std;

int main() {
    unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0, blankCnt = 0, tabCnt = 0, newlineCnt = 0;

    char ch;

    while (cin >> noskipws >> ch) {
        switch (ch) {
            case 'a':
            case 'A':
                aCnt++;
                break;
            case 'e':
            case 'E':
                eCnt++;
                break;
            case 'i':
            case 'I':
                iCnt++;
                break;
            case 'o':
            case 'O':
                oCnt++;
                break;
            case 'u':
            case 'U':
                uCnt++;
                break;
            default:
                break;
        }
    }

    const auto total = aCnt + eCnt + iCnt + oCnt + uCnt;
    cout << "Count of a: " << aCnt << endl
            << "Count of e: " << eCnt << endl
            << "Count of i: " << iCnt << endl
            << "Count of o: " << oCnt << endl
            << "Count of u: " << uCnt << endl
            << "Total vowels: " << total << endl;
}
```

## Exercise 5.11

### Question:

Modify our vowel-counting program so that it also counts the number of blank spaces, tabs, and newlines read.

### Answer:

```cpp
#include <iostream>

using namespace std;

int main() {
    unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0, blankCnt = 0, tabCnt = 0, newlineCnt = 0;

    char ch;

    while (cin >> noskipws >> ch) {
        switch (ch) {
            case 'a':
            case 'A':
                aCnt++;
                break;
            case 'e':
            case 'E':
                eCnt++;
                break;
            case 'i':
            case 'I':
                iCnt++;
                break;
            case 'o':
            case 'O':
                oCnt++;
                break;
            case 'u':
            case 'U':
                uCnt++;
                break;
            case ' ':
                blankCnt++;
                break;
            case '\t':
                tabCnt++;
                break;
            case '\n':
            case '\r':
                newlineCnt++;
                break;
            default:
                break;
        }
    }

    const auto total = aCnt + eCnt + iCnt + oCnt + uCnt;
    cout << "Count of a: " << aCnt << endl
            << "Count of e: " << eCnt << endl
            << "Count of i: " << iCnt << endl
            << "Count of o: " << oCnt << endl
            << "Count of u: " << uCnt << endl
            << "Total vowels: " << total << endl
            << "Blank spaces: " << blankCnt << endl
            << "Tabs : " << tabCnt << endl
            << "New lines: " << newlineCnt << endl;
}
```

## Exercise 5.12

### Question:

Modify our vowel-counting program so that it counts the number of occurrences of the following two-character sequences

### Answer:

```cpp
#include <iostream>

using namespace std;

int main() {
    unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0, blankCnt = 0, tabCnt = 0, newlineCnt = 0, sequencesCnt =
            0;

    char ch;
    char previous = '\0';

    while (cin >> noskipws >> ch) {
        switch (ch) {
            case 'a':
            case 'A':
                aCnt++;
                break;
            case 'e':
            case 'E':
                eCnt++;
                break;
            case 'i':
            case 'I':
                if (previous == 'f') sequencesCnt++;
                iCnt++;
                break;
            case 'o':
            case 'O':
                oCnt++;
                break;
            case 'u':
            case 'U':
                uCnt++;
                break;
            case ' ':
                blankCnt++;
                break;
            case '\t':
                tabCnt++;
                break;
            case '\n':
            case '\r':
                newlineCnt++;
                break;
            case 'f':
            case 'l':
                if (previous == 'f') sequencesCnt++;
            default:
                break;
        }

        previous = ch;
    }

    const auto total = aCnt + eCnt + iCnt + oCnt + uCnt;
    cout << "Count of a: " << aCnt << endl
            << "Count of e: " << eCnt << endl
            << "Count of i: " << iCnt << endl
            << "Count of o: " << oCnt << endl
            << "Count of u: " << uCnt << endl
            << "Total vowels: " << total << endl
            << "Blank spaces: " << blankCnt << endl
            << "Tabs : " << tabCnt << endl
            << "New lines: " << newlineCnt << endl
            << "Sequences: " << sequencesCnt << endl;
}
```

## Exercise 5.13

### Question:

Each of the programs in the highlighted text on page 184 contains a common programming error. Identify and correct each error.

```cpp
// (a)
unsigned aCnt = 0, eCnt = 0, iouCnt = 0;
char ch = next_text();
switch (ch) {
    case 'a': aCnt++;
    case 'e': eCnt++;
    default: iouCnt++;
}
// (b)
unsigned index = some_value();
switch (index) {
    case 1:
        int ix = get_value();
        ivec[ ix ] = index;
        break;
    default:
        ix = ivec.size()-1;
        ivec[ ix ] = index;
}
// (c)
unsigned evenCnt = 0, oddCnt = 0;
int digit = get_num() % 10;
switch (digit) {
    case 1, 3, 5, 7, 9:
        oddcnt++;
        break;
    case 2, 4, 6, 8, 10:
        evencnt++;
        break;
}
// (d)
unsigned ival=512, jval=1024, kval=4096;
unsigned bufsize;
unsigned swt = get_bufCnt();
switch(swt) {
    case ival:
        bufsize = ival * sizeof(int);
        break;
    case jval:
        bufsize = jval * sizeof(int);
        break;
    case kval:
        bufsize = kval * sizeof(int);
        break;
}
```

### Answers:

```cpp
// (a)
unsigned aCnt = 0, eCnt = 0, iouCnt = 0;
char ch = next_text();
switch (ch) {
    case 'a':
        aCnt++;
        break;
    case 'e':
        eCnt++;
        break;
    default:
        iouCnt++;
        break;
}
// (b)
unsigned index = some_value();
switch (index) {
    case 1: {
        int ix = get_value();
        ivec[ ix ] = index;
        break;
    }
    default:
        auto ix = ivec.size()-1;
        ivec[ ix ] = index;
}
// (c)
unsigned evenCnt = 0, oddCnt = 0;
int digit = get_num() % 10;
switch (digit) {
    case 1:
    case 3:
    case 5:
    case 7:
    case 9:
        oddCnt++;
        break;
    case 2:
    case 4:
    case 6:
    case 8:
    case 10:
        evenCnt++;
        break;
    default:
        break;
}
// (d)
constexpr unsigned ival=512, jval=1024, kval=4096;
unsigned bufsize;
unsigned swt = get_bufCnt();
switch (swt) {
    case ival:
        bufsize = ival * sizeof(int);
        break;
    case jval:
        bufsize = jval * sizeof(int);
        break;
    case kval:
        bufsize = kval * sizeof(int);
        break;
    default:
        break;
}
```

# Exercise Section 5.4.1

## Exercise 5.14

### Question:

Write a program to read strings from standard input looking for duplicated words. The program should find places in the input where one word is followed immediately by itself. 

Keep track of the largest number of times a single repetition occurs and which word is repeated. Print the maximum number of duplicates, or else print a message saying that no word was repeated.

For example, if the input is
> how now now now brown cow cow

the output should indicate that the word nowoccurred three times.

### Answer:

```cpp
string duplicated_word;
unsigned duplicated_count = 0;

unsigned count = 0;

for (string input, previous; cin >> input; previous = input) {
    if (input == previous) {
        count++;
    } else {
        count = 1;
    }

    if (count > duplicated_count) {
        duplicated_word = input;
        duplicated_count = count;
    }
}

if (duplicated_count < 1)
    cout << "No duplicates found" << endl;
else {
    cout << "The word \"" << duplicated_word << "\" occurred " << duplicated_count << " times" << endl;
}
```

# Exercise Section 5.4.2

## Exercise 5.15

### Question:

Explain each of the following loops. Correct any problems you detect.

a.
```cpp
for (int ix = 0; ix != sz; ++ix) { /* ... */ }
if (ix != sz)
    // ...
```

b.
```cpp
int ix;
for (ix != z; ++ix) { /* ... */ }
```

c.
```cpp
for (int ix = 0; ix != sz; ++ix, ++sz) { /* ... */ }
```

### Answers:

a. Initialization declarator moved outside of the for statement
```cpp
const int sz = 20;
int ix = 0;

for (; ix != sz; ++ix) { /* ... */ }
if (ix != sz)
    ;
```

b. Null-statement added before condition

```cpp
int ix = 0;
int z = 5;
for (; ix != z; ++ix) { /* ... */ }
```

c. Indefinitely running loop - remove `++sz` from expression

```cpp
for (int ix = 0; ix != sz; ++ix) { /* ... */ }
```

## Exercise 5.16

### Question:

The `while` loop is particularly good at executing while some condition holds; for example, when we need to read values until end-of-file. The `for` loop is generally thought of as a step loop: An index steps through a range of values in a collection. Write an idiomatic useof each loop and then rewrite each using the other loop construct. If you could use only one loop, which would you choose? Why?

### Answers:

```cpp
// idiomatic while
std::string word;
while (std::cin >> word)
    ;

// idiomatic for
unsigned x = 10;
for (unsigned i = 0; i != x; i++)
    ;

// while
unsigned y = 10;
unsigned j = 0;
while (j != y)
    j++;

// for
for (std::string word; std::cin >> word;) 
    ;
```

## Exercise 5.17

### Question:

Given two vectors of ints, write a program to determine whether one vector is a prefix of the other. For vectors of unequal length, compare the number of elements of the smaller vector. For example, given the vectors containing `0, 1, 1, 2` and `0, 1, 1, 2, 3, 5, 8`, respectively your program should return `true`

### Answer:

```cpp
bool is_prefix(std::vector<int> &left, std::vector<int> &right) {
    if (left.size() > right.size())
        return is_prefix(right, left);

    for (decltype(left.size()) i = 0; i < left.size(); i++) {
        if (left[i] != right[i]) return false;
    }

    return true;
}

int main() {
    std::vector<int> vec1{0, 1, 1, 2};
    std::vector<int> vec2{0, 1, 1, 2, 3, 5, 8};

    const bool prefix = is_prefix(vec1, vec2);

    std::cout << "Is prefix: " << prefix << std::endl;
}
```

# Exercise Section 5.4.4

## Exercise 5.18

### Question:

Explain each of the following loops. Correct any problems you detect.

a.
```cpp
do
    int v1, v2;
    cout << "Please enter two numbers to sum:" ;
    if (cin >> v1 >> v2)
        cout << "Sum is: " << v1 + v2 << endl;
while(cin);
```

b.
```cpp
do {

} while (int ival = get_response());
```

c.
```cpp
do {
    int ival = get_response();
} while (ival);
```

### Answers:

a. This loop reads two integers from the input stream which it then sums together. This code snippet is missing a block statement to encapsulate the body of the loop.

```cpp
do {
    int v1, v2;
    cout << "Please enter two numbers to sum:";
    if (cin >> v1 >> v2)
        cout << "Sum is: " << v1 + v2 << endl;
}
while(cin);
```

b. This loop attempts to declare a variable within the condition which is not allowed since the loop statement is invoked first before the condition is tested. Allowing this would allow the statement to access a non-initialized variable.

There are two potential solutions:
```cpp
int ival = 0;
do {
    // This may not make sense depending on the context since ival will be 0 in the first iteration
} while (ival = get_response());
```

alternatively:
```cpp
while (int ival = get_response()) {
    // ival will be initialized for the first iteration   
}
```

c. This loop attempts to use a variable defined within the statement block as a loop condition.

There are also two possible solutions here:

```cpp
int ival; // Declare ival outside of the statement block
do {
    ival = get_response();
} while (ival);
```

Alternatively, similarly to scenario b.

```cpp
while (int ival = get_response()) {
    // ival will be initialized for the first iteration   
}
```

# Exercise Section 5.5.1

## Exercise 5.20

### Question:

Write a program to read a sequence of strings from the standard input until either the same word occurs twice in succession or all the words have been read. Use a `while` loop to read the text one word at a time. Use the `break` statement to terminate the loop if a word occurs twice in succession. Print the word if it occurs twice in succession, or else print a message saying that no word was repeated.

### Answer:

```cpp
std::string previous, current;
bool duplicate_found = false;
while (std::cin >> current) {
    if (previous == current) {
        std::cout << "Duplicate word found: " << current << std::endl;
        duplicate_found = true;
        break;
    }

    previous = current;
}

if (!duplicate_found) {
    std::cout << "No duplicate found" << std::endl;
}
```

# Exerise Section 5.5.2

## Exercise 5.21

### Question:

Revise the program from the exercise in § 5.5.1 so that it looks only for duplicated words that start with an uppercase letter.

### Answer:

```cpp
std::string previous, current;
bool duplicate_found = false;
while (std::cin >> current) {
    if (!std::isupper(current[0]))
        continue;

    if (previous == current) {
        std::cout << "Duplicate word found: " << current << std::endl;
        duplicate_found = true;
        break;
    }

    previous = current;
}

if (!duplicate_found) {
    std::cout << "No duplicate found" << std::endl;
}
```

# Exerise Section 5.5.3

## Exercise 5.22

### Question:

The last example in this section that jumped back to `begin` could be better written using a loop. Rewrite the code to eliminate the `goto`

### Answer:

```cpp
while (int sz = get_size()) {
    if (sz > 0)
        break;
}
```

# Exercise Section 5.6.3

## Exercise 5.23

### Question:

Write a program that reads two integers from the standard input and prints the result of dividing the first number by the second.

### Answer:

```cpp
int first, second;

std::cout << "Enter two numbers: ";

std::cin >> first >> second;

std::cout << "Result: " << first / second << std::endl;
```

## Exercise 5.24

### Question:

Revise your program to throw an exception if the second number is zero. Test your program with a zero input to see what happens on your system if you don't `catch` an exception

### Answer:

```cpp
int first, second;

std::cout << "Enter two numbers: ";

std::cin >> first >> second;

if (second == 0)
    throw std::out_of_range("Second value was zero");

std::cout << "Result: " << first / second << std::endl;
```

## Exercise 5.25

### Question:

Revise your program from the previous exercise to use a `try` block to `catch` the exception. The `catch` clause should print a message to the user to supply a new number and repeat the code inside the `try`

### Answer:

```cpp
int first, second;
while (true) {
    try {
        std::cout << "Enter two numbers: ";

        std::cin >> first >> second;

        if (second == 0)
            throw std::out_of_range("Second value was zero");

        std::cout << "Result: " << first / second << std::endl;
    } catch (std::out_of_range const &error) {
        std::cout << error.what() << std::endl;
        std::cout << "Try again? Y or N: ";

        char c;
        std::cin >> c;
        if (!std::cin || c == 'n')
            break;
    }
}
```