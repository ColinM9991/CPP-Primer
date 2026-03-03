# Exercise Section 7.1.1

## Exercise 7.1

### Question

Write a version of the transaction-processing program from § 1.6 using the `Sales_data` class you defined for the exercises in § 2.6.1

### Answer

```cpp
#include <iostream>

struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main() {
    Sales_data total;
    double price;
    if (std::cin >> total.bookNo >> total.units_sold >> price) {
        total.revenue = price * total.units_sold;
        Sales_data trans;
        while (std::cin >> trans.bookNo >> trans.units_sold >> price) {
            if (total.bookNo == trans.bookNo) {
                total.units_sold += trans.units_sold;
                total.revenue += price * trans.units_sold;
            } else {
                std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " " << total.revenue /
                        total.units_sold << std::endl;
                total = trans;
            }
        }
        std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " " << total.revenue / total.
                units_sold << std::endl;
    } else {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

# Exercise Section 7.1.2

## Exercise 7.2

### Question

Add the `combine` and `isbn` members to the `Sales_data` class you wrote for the exercises in § 2.6.2

###  Answer

```cpp
struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;

    std::string isbn() const { return bookNo; }

    Sales_data& combine(const Sales_data &rhs) {
        units_sold += rhs.units_sold;
        revenue += rhs.revenue;

        return *this;
    }
};

```

## Exercise 7.3

### Question

Revise your transaction-processing program from § 7.1.1 to use these members

### Answer

```cpp
#include <iostream>

struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;

    std::string isbn() const { return bookNo; }

    Sales_data& combine(const Sales_data &rhs) {
        units_sold += rhs.units_sold;
        revenue += rhs.revenue;

        return *this;
    }
};

int main() {
    Sales_data total;
    double price;
    if (std::cin >> total.bookNo >> total.units_sold >> price) {
        total.revenue = price * total.units_sold;
        Sales_data trans;
        while (std::cin >> trans.bookNo >> trans.units_sold >> price) {
            trans.revenue = price * trans.units_sold;
            if (total.isbn() == trans.isbn()) {
                total.combine(trans);
            } else {
                std::cout << total.isbn() << " " << total.units_sold << " " << total.revenue << " " << total.revenue /
                        total.units_sold << std::endl;
                total = trans;
            }
        }
        std::cout << total.isbn() << " " << total.units_sold << " " << total.revenue << " " << total.revenue / total.
                units_sold << std::endl;
    } else {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

## Exercise 7.4

### Question

Write a class named `Person` that represents the name and address of a person. Use a `string` to hold each of these elements. Subsequent exercises will incrementally add features to this class

### Answer

```cpp
struct Person {
    std::string name;
    std::string address;
};
```

## Exercise 7.5

### Question

Provide operations in your `Person` class to return the name and address. Should these functions be `const`? Explain your choice.

### Answer

```cpp
struct Person {
    std::string name;
    std::string address;

    std::string getName() const { return name; }
    std::string getAddress() const { return address; }
};
```

These functions are `const` because they do not modify the object which they operate on.

# Exercise Secftion 7.1.3

## Exercise 7.6

### Question

Define your own versions of the `add`, `read`, and `print` functions.

### Answer

```cpp
std::istream &read(std::istream &is, Sales_data &item) {
    double price;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = item.units_sold * price;

    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item) {
    os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs) {
    Sales_data sum = lhs;
    sum.combine(rhs);

    return sum;
}
```

## Exercise 7.7

### Question

Rewrite the transaction-processing program you wrote for the exercises in § 7.1.2

### Answer

```cpp
int main() {
    Sales_data total;
    if (read(std::cin, total)) {
        Sales_data trans;
        while (read(std::cin, trans)) {
            if (total.isbn() == trans.isbn()) {
                total = add(total, trans);
            } else {
                print(std::cout, total) << std::endl;
                total = trans;
            }
        }
        print(std::cout, total) << std::endl;
    } else {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

## Exercise 7.8

### Question

Why does `read` define its `Sales_data` parameter as a plain reference and `print` define its parameter as a reference to `const`?

### Answer

`print` uses `const Sales_data &` because it does not mutate the state of the `Sales_data` object, it simply prints the values to the output stream, while `read` does mutate the `Sales_data` reference.

## Exercise 7.9

### Question

Add operations to read and print `Person` objects to the code you wrote for the exercises in § 7.1.2

### Answer

```cpp
struct Person {
    std::string name;
    std::string address;
};

std::istream &read(std::istream &is, Person &item) {
    std::getline(std::cin, item.name);
    std::getline(std::cin, item.address);

    return is;
}

std::ostream &print(std::ostream &os, const Person &item) {
    os << item.name << " " << item.address;

    return os;
}
```

## Exercise 7.10

### Question

What does the condition in the following `if` statement do?

```cpp
if (read(read(cin, data1), data2))
```

### Answer

The condition first invokes `read(cin, data1)` which returns the `std::istream` buffer that is subsequently used to invoke `read(std::istream, data2)`.

The contents of the stream may be consumed (depending on the implementation of `read`) to populate `data1`, thus the remaining contents may then be used to populate `data2`.

The condition checks whether `read(std::cin, data2)` was successful and does not validate the nested read.

# Exercise Section 7.1.4

## Exercise 7.11

### Question

Add constructors to your `Sales_data` class and write a program to use each of the constructors

### Answer

```cpp
struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;

    Sales_data() = default;
    Sales_data(const std::string &bookNo) : bookNo(bookNo) {}
    Sales_data(const std::string &bookNo, unsigned unitsSold, unsigned price) : bookNo(bookNo), units_sold(unitsSold), revenue(price * unitsSold) {}
    Sales_data(std::istream &is);
};

std::istream &read(std::istream &is, Sales_data &item) {
    double price;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = item.units_sold * price;

    return is;
}

Sales_data::Sales_data(std::istream &is) {
    read(is, *this);
}

int main() {
    Sales_data first(std::cin);
    Sales_data second(std::string("Test"));
    Sales_data third(std::string("Test"), 5, 10);
    Sales_data fourth;
}
```

## Exercise 7.12

### Question

Move the definition of the `Sales_data` constructor that takes an `std::istream` into the body of the `Sales_data` class

### Answer

```cpp
struct Sales_data;
std::istream &read(std::istream &is, Sales_data &item);

struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;

    Sales_data() = default;
    Sales_data(const std::string &bookNo) : bookNo(bookNo) {}
    Sales_data(const std::string &bookNo, unsigned unitsSold, unsigned price) : bookNo(bookNo), units_sold(unitsSold), revenue(price * unitsSold) {}
    Sales_data(std::istream &is) {
        read(is, *this);
    }
};
```

## Exercise 7.13

### Question

Rewrite the program from page 255 to use the `std::istream` constructor

### Answer

```cpp
int main() {
    Sales_data total(std::cin);
    if (!total.isbn().empty()) {
        Sales_data trans;
        while (trans = Sales_data(std::cin), !trans.isbn().empty()) {
            if (total.isbn() == trans.isbn()) {
                total = add(total, trans);
            } else {
                print(std::cout, total) << std::endl;
                total = trans;
            }
        }
        print(std::cout, total) << std::endl;
    } else {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

# Exercise Section 7.2

## Exercise 7.16

### Question

What, if any, are the constraints on where and how often an access specifier may appear inside a class definition? What kinds of members should be defined after a `public` specifier? What kinds should be `private`?

### Answer

A class can contain zero or more access specifiers and there's no limit on how many times an access specifier can appear.

Members which are intended to be accessible throughout the program should be defined within the `public` access specifier. Otherwise, they should be defined within the `private` specifier.

## Exercise 7.17

### Question

What, if any, are the differences between using `class` or `struct`?

### Answer

When using `class`, members are `private` by default. With `struct`, members are `public` by default.

## Exercise 7.18

### Question

What is encapsulation? Why is it useful?

### Answer

Encapsulation is the process of restricting access to members and enforcing strict usage of a class declaration. This is useful as it prevents misuse of members.

## Exercise 7.19

### Question

Indicate which members of your `Person` class you would declare as `public` and which you would declare as `private`. Explain your choice.

### Answer

`name` and `address` should be marked as `private`. The constructors, as well as some `const` functions for `getName` and `getAddress` would be `public`.

# Exercise Section 7.2.1

## Exercise 7.20

### Question

When are friends useful? Discuss the pros and cons of using friends

### Answer

`friend` is useful when a function declaration exists outside of the class but is an operation on the class.

## Exercuse 7.21

### Question

Update your `Sales_data` class to hide its implementation. The programs you've written to use `Sales_data` operations should still continue to work. Recompile those programs with your new class definition to verify that they still work.

### Answer

```cpp
struct Sales_data {
    friend std::istream &read(std::istream &is, Sales_data &item);
    friend std::ostream &print(std::ostream &os, const Sales_data &item);
private:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;

public:
    Sales_data() : units_sold(0), revenue(0.0) {};
    Sales_data(const std::string &bookNo) : bookNo(bookNo) {}
    Sales_data(const std::string &bookNo, unsigned unitsSold, unsigned price) : bookNo(bookNo), units_sold(unitsSold), revenue(price * unitsSold) {}
    Sales_data(std::istream &is) {
        read(is, *this);
    }

    const std::string &isbn() const { return bookNo; }

    Sales_data &combine(const Sales_data &rhs) {
        units_sold += rhs.units_sold;
        revenue += rhs.revenue;

        return *this;
    }

    double avg_price() const { return revenue / units_sold; }
};
```

## Exercise 7.22

### Question

Update your `Person` class to hide its implementation

### Answer

```cpp
struct Person {
    friend std::ostream &print(std::ostream &, const Person &);
    friend std::istream &read(std::istream &, Person &);
private:
    std::string name;
    std::string address;

public:
    Person() = default;
    Person(const std::string &name, const std::string &address) : name(name), address(address) {}
    Person(const std::istream &is) {;}

    const std::string &getName() const { return name; }
    const std::string &getAddress() const { return address; }
};

std::istream &read(std::istream &is, Person &item) {
    std::getline(std::cin, item.name);
    std::getline(std::cin, item.address);

    return is;
}
```

# Exercise Section 7.3.1

## Exercise 7.23

### Question

Write your own version of the `Screen` class

### Answer

```cpp
class Screen {
public:
    using pos = std::string::size_type;

    Screen() = default;

    Screen(pos ht, pos wd, char c) : height(ht), width(wd), contents(ht * wd, c) {
    }

    char get() const {
        return contents[cursor];
    }

    char get(pos ht, pos wd) const {
        pos row = ht * width;
        return contents[row + wd];
    }

    Screen &move(pos r, pos c) {
        pos row = r * width;
        cursor = row + c;
        return *this;
    }

private:
    pos cursor = 0;
    pos height = 0, width = 0;
    std::string contents;
};
```

## Exercise 7.24

### Question

Give your `Screen` class three constructors: a default constructor; a constructor that takes values for height and width and initializes the contents to hold the given number of blanks; and a constructor that takes values for height, width, and a character to use as the contents of the screen.

### Answer

```cpp
using pos = std::string::size_type;
Screen() = default;

Screen(pos ht, pos wd) : height(ht), width(wd), contents(ht * wd, ' ') {
}

Screen(pos ht, pos wd, char c) : height(ht), width(wd), contents(ht * wd, c) {
}
```

## Exercise 7.25

### Question

Can `Screen` safely rely on the default versions of copy and assignment? If so, why? If not, why not;

### Answer

`Screen` can rely on thedefault versions of copy and assignment because it uses built-in and `std::string` member types.

## Exerise 7.26

### Question

Define `Sales_data::avg_price` as an `inline` function

### Answer

```cpp
inline double Sales_data::avg_price() const { return revenue / units_sold; }
```

# Exercise Section 7.3.2

## Exercise 7.27

### Question

Add the `move`, `set` and `display` operations to your version of `Screen`. Test your class by executing the following code

```cpp
Screen myScreen(5, 5, 'X');
myScreen.move(4, 0).set('#').display(std::cout);
std::cout << std::endl;
myScreen.display(std::cout);
std::cout << std::endl;
```

### Answer

```cpp
inline Screen &Screen::move(pos r, pos c) {
    const pos row = r * width;
    cursor = row + c;
    return *this;
}

inline Screen &Screen::set(char c) {
    contents[cursor] = c;

    return *this;
}

inline Screen &Screen::set(pos r, pos col, char c) {
    contents[r * width + col] = c;

    return *this;
}

inline Screen &Screen::display(std::ostream &os) {
    os << contents;

    return *this;
}

inline const Screen &Screen::display(std::ostream &os) const {
    os << contents;

    return *this;
}

int main() {
    Screen myScreen(5, 5, 'X');
    myScreen.move(4, 0).set('#').display(std::cout);
    std::cout << std::endl;
    myScreen.display(std::cout);
    std::cout << std::endl;
}
```

## Exercise 7.28

### Question

What would happen in the previous exercise if the return type of `move`, `set` and `display` was `Screen` rather than `Screen&`?

### Answer

`move`, `set` and `display` would return a copy of `Screen`.  

0. `move` moves the cursor position on `myScreen` and returns a copy. 
0. `Screen` copy contains the copied cursor position and buffer. 
0. `set` sets the character of the copied `Screen` returns another copy. 
0. `Screen` copy contains the copied cursor position and buffer. 
0. `display` displays the output of the latest copy `XXXXXXXXXXXXXXXXXXXX#XXX`. 
0. `myScreen` characters remain unchanged. Cursor position is still `(4, 0)`, buffer `XXXXXXXXXXXXXXXXXXXXXXXXX`. 

It would be equivalent to writing the following code:
```cpp
Screen myScreen(5, 5, 'X');
Screen temp = myScreen.move(4, 0);
Screen temp2 = temp.set('#');
Screen temp3 = temp.display();
```

## Exercise 7.29

### Question

Revise your `Screen` class so that `move`, `set` and `display` functions return `Screen` and check your prediction from the previous exercise.

### Answer

```cpp
inline Screen Screen::move(pos r, pos c) {
    const pos row = r * width;
    cursor = row + c;
    return *this;
}

inline Screen Screen::set(char c) {
    contents[cursor] = c;

    return *this;
}

inline Screen Screen::display(std::ostream &os) {
    os << contents;

    return *this;
}

int main() {
    Screen myScreen(5, 5, 'X');
    myScreen.move(4, 0).set('#').display(std::cout);
    std::cout << std::endl;
    myScreen.display(std::cout);
    std::cout << std::endl;
}
```

## Exercise 7.30

### Question

It is legal but redundant to refer to members through the `this` pointer. Discuss the pros and cons of explicitly using the `this` pointer to access members

### Answer

Pros:

* Allows you to use parameter names with the same name as a member field

Cons:
* Unnecessary in many scenarios

# Exercise Section 7.3.3

## Exercise 7.31

### Question

Define a pair of classes `X` and `Y`, in which `X` has a pointer to `Y`, and `Y` has an object of type `X`.

### Answer

```cpp
class Y;
class X {
    Y *y;
};

class Y {
    X x;
};
```

# Exercise Section 7.3.4

## Exercise 7.32

### Question

Define your own versions of `Screen` and `Window_mgr` in which `clear` is a member of `Window_mgr` and a friend of `Screen`

### Answer

```cpp
class Screen;
class Window_mgr {
public:
    using ScreenIndex = std::vector<Screen>::size_type;
    void clear(ScreenIndex);
private:
    std::vector<Screen> screens;
};
class Screen {
    friend void Window_mgr::clear(Window_mgr::ScreenIndex);
}

void Window_mgr::clear(Window_mgr::ScreenIndex i) {
    if (i >= screens.size()) return;

    Screen &s = screens[i];
    s.contents = std::string(s.height * s.width, ' ');
}
```

# Exercise Section 7.4

## Exercise 7.33

### Question

What would happen if we gave `SCreen` a `size` memebr defined as follows? Fix any problems you identify.

```cpp
pos Screen::size() const {
    return height * width;
}
```

### Answer

This would fail to compile because the return type, `pos`, is outside of the class body and thus is outside the class scope.

To correct this, the class scope needs to be included.

```cpp
inline Screen::pos Screen::size() const {
    return height * width;
}
```

# Exercise Section 7.4.1

## Exercise 7.34

### Question

What would happen if we put the `typedef` of `pos` in the `Screen` class on page 285 as the last line in the class?

### Answer

The compiler would fail to compile the application as `typedef` or type aliases need to be defined first in order to be resolvable

## Exercise 7.35

### Question

Explain the following code, indicating which definition of `Type` or `initVal` is used for each use of those names. Say how you would fix any errors.

```cpp
typedef string Type;
Type initVal();
class Exercise {
public:
    typedef double Type;
    Type setVal(Type);
    Type initVal();
private:
    int val;
}

Type Exercise::setVal(Type parm) {
    val = parm + initVal();
    return val;
}
```

### Answer

The return type of `Type` would be of type `std::string`, as it is outside of the class scope, while the parameter `Type` would be of type `double`.

`Exercise::initVal` will be used as the scope is within the class scope.

To resolve:

```cpp
class Exercise {
public:
    typedef double Type;
    Type setVal(Type);
    Type initVal();
private:
    int val;
};

Exercise::Type Exercise::setVal(Type parm) {
    val = parm + initVal();
    return val;
}
```

# Exercise Section 7.5.1

## Exercise 7.36

### Question

The following initializer is in error. Identify and fix the problem

```cpp
struct X {
    X (int i, int j) : base(i), rem(base % j) { };
    int rem, base;
}
```

### Answer

```cpp
struct X {
    X (int i, int j) : base(i), rem(i % j) { };
    int rem, base;
}
```

## Exercise 7.37

### Question

Using the version of `Sales_data` from this section, determine which constructor is used to initialize each of the following variables and list the values of the data members in each object:

```cpp
Sales_data first_item(std::cin);

int main() {
    Sales_data next;
    Sales_data last("9-999-99999-9");
}
```

### Answer

```cpp
Sales_data first_item(std::cin); // Sales_data(std::istream &);

int main() {
    Sales_data next; // Sales_data(const std::string) : bookNo(""), units_sold(0), revenue(0)
    Sales_data last("9-999-99999-9"); // Sales_data(const std::string) : bookNo("9-999-99999-9"), units_sold(0), revenue(0) 
}
```

## Exercise 7.39

### Question

We might want to supply `std::cin` as a default argument ot the constructor that takes an `std::istream&`. Write the constructor declaration that uses `std::cin` as a default argument

### Answer

```cpp
Sales_data(std::istream &is = std::cin)
```

## Exercise 7.39

### Question

Would it be legal for both the constructor that takes a `std::string` and the one that takes an `std::istream&` to have default arguments? If not, why not?

### Answer

It would not be legal because it creates an ambiguous call in which the program will be unable to know whether `Sales_data(std::string)` or `Sales_data(std::istream &)` should be used.

## Exercise 7.40

### Question

Choose one of hte following abstractions (or an abstraction of your own choosing). Determine what data are needed in the class. Provide an appropriate set of constructors. Explain your decisions.

### Answer

```cpp
class Employee {
private:
    unsigned id;
    std::string name;
    std::string role;
public:
    Employee(unsigned id, std::string name, std::string role) : id(id), name(name), role(role) {
    }

    Employee(unsigned id, std::string name) : id(id), name(name), role("") {
    }

    Employee() = default;

    Employee(std::istream &is) {
        is >> id >> name >> role;
    }
};
```

# Exercise Section 7.5.2

## Exercise 7.41

### Question

Rewrite your own version of the `Sales_data` class to use delegating constructors. Add a statement to the body of each of the constructors that prints a message whenever it is executed. Write declarations to construct a `Sales_data` object in every way possible. Study the output until you are certain you understand the order of execution among delegating constructors.

### Answer

```cpp
Sales_data(const std::string &bookNo, unsigned unitsSold, unsigned price) : bookNo(bookNo), units_sold(unitsSold),
    revenue(price * unitsSold) {
    std::cout << "Base ctor" << std::endl;
    // Base Ctor
}

Sales_data() : Sales_data("", 0, 0) {
    std::cout << "Default ctor" << std::endl;
    // Base Ctor - Default Ctor
};

Sales_data(const std::string &) : Sales_data(bookNo, 0, 0) {
    std::cout << "Copy ctor" << std::endl;
    // Base Ctor - Copy Ctor
}

Sales_data(std::istream &is) : Sales_data() {
    std::cout << "Stream ctor" << std::endl;
    read(is, *this);
    // Base Ctor - Default Ctor - Stream Ctor
}
```

## Exercise 7.42

### Question

For the class you wrote for exercise 7.40 in § 7.5.1, decide whether any of the constructors might use delegation. If so, write the delegating constructor(s) for your class. If not, look at the list of abstractions and choose one that you think would use a delegating constructor. Write the class definition for that abstraction

### Exercise

```cpp
class Employee {
private:
    unsigned id;
    std::string name;
    std::string role;
public:
    Employee(unsigned id, std::string name, std::string role) : id(id), name(name), role(role) {
    }

    Employee(unsigned id, std::string name) : Employee(id, name, "") {
    }

    Employee() : Employee(0, "", "") {};

    Employee(std::istream &is) : Employee() {
        is >> id >> name >> role;
    }
};
```

# Exercise Section 7.5.3

## Exercise 7.43

### Question

Assume we have a class named `NoDefault` that has a constructor that takes an `int`, but has no default constructor. Define a class `C` that has a member of type `NoDefault`. Define the default constructor for C.

### Answer

```cpp
class NoDefault {
public:
    NoDefault(int) {};
};

class C {
public:
    C() : n(0) {};
private:
    NoDefault n;
};
```

## Exercise 7.44

### Question

Is the following declaration legal? If not, why not?

```cpp
std::vector<NoDefault> vec(10);
```

### Answer

The declaration is not legal because it uses the `std::vector<NoDefault>(size_type)` constructor of `std::vector<NoDefault>`. `NoDefault` does not have a default constructor.

The correct declaration would be
```cpp
std::vector<NoDefault> v(10, NoDefault(0));
```

## Exercise 7.45

### Question

What if we defined the `vector` in the previous exercise to hold objects of type `C`?

### Answer

This would be a valid declaration since `C` has a default constructor

## Exercise 7.46

### Question

Which, if any, of the following statements are untrue? Why?

<ol type="a">
  <li>A class must provide at least one constructor.</li>
  <li>A default constructor is a constructor with an empty parameter list.</li>
  <li>If there are no meaningful default values for a class, the class should not provide a default constructor.</li>
  <li>If a class does not define a default constructor, the compiler generates one that initializes each data member to the default value of its associated type.</li>
</ol>

### Answer

A: The compiler will generate a synthesized constructor if one has not been created.  
B: A default constructor is a constructor that does not accept parameters. A parameterized constructor may also be a default if all parameters are value initialized 
D: Partially incorrect. Members of a built-in or compound type have an undefined value when default initialized. This will need to be in-class initialized. A synthesized constructor will initialize member types that also have a synthesized default constructor

# Exercise Section 7.5.4

## Exercise 7.47

### Question

Explain whether the `Sales_data` constructor that takes a `string` should be `explicit`. What are the benefits of making the constructor `explicit`? What are the drawbacks?

### Answer

The constructor can optionally be explicit depending on the intent.

The benefit is that it may allow the caller to reuse any temporary `Sales_data` object that's created for any function declaration expecting `Sales_data`, where this temporary would be discarded after such functions have finished executing.

The drawback might be less flexibility but the flexibility isn't that great.

## Exercise 4.78

### Question

Assuming the `Sales_data` constructors are not `explicit`, what operations happen during the following definitions

```cpp
std::string null_isbn("9-999-99999-9");
Sales_data item1(null_isbn);
Sales_data item2("9-999-99999-9");
```

What happens if the `Sales_data` constructors are explicit?

### Answer

```cpp
std::string null_isbn("9-999-99999-9");
Sales_data item1(null_isbn); // Calls constructor with signature `Sales_data(const std::string &)
Sales_data item2("9-999-99999-9"); // Implicit conversion from const char* to std::string
```

No difference if all constructors are marked as explicit.

## Exercise 7.49

### Question

For each of the three following declarations of `combine`, explain what happens if we call `i.combine(s)`, where `i` is a `Sales_data` and `s` is a `std::string`

0. `Sales_data &combine(Sales_data);`
0. `Sales_data &combine(Sales_data&);`
0. `Sales_data &combine(const Sales_data&) const;`

### Answer

0. Implicit conversion from `std::string` to `Sales_data`
0. Compilation error, Non-const lvalue reference to type Sales_data cannot bind to lvalue of type std::string
0. `combine` mutates state. It cannot be a `const` function.

## Exercise 7.50

### Question

Determine whether any of your `Person` class constructors should be `explicit

### Answer

The folowing constructor can be marked as explicit
```cpp
Person(const std::istream &is)
```

## Exercise 7.51

### Question

Why do you think `std::vector` defines its single-argument constructor as `explicit`, but `string` does not?

### Answer

There is very little confusion to be had with the following code:

```cpp
void function(const std::string &);

function("Hello, world");
```

On the other hand, had `std::vector(size_type)` not been marked as `explicit` then this would be possible

```cpp
void function(const std::vector<std::string> &);

function(42);
```

The result is a confusing API that doesn't become immediately clear what 42 represents.

# Exercise Section 7.5.5

## Exercise 7.52

### Question

Using our first version of `Sales_data` from § 2.6.1, explain the following initialization. Identify and fix any problems

```cpp
struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
}

Sales_data item = { "978-0590353403", 25, 15.99 };
```

### Answer

As per the rules of an aggregate class, they can be brace initialized when:  

0. All members are public
0. There are no constructors
0. It has no in-class initializers
0. There are no base classes or virtual functions

The earlier implementation of `Sales_data` contains in-class initializers which must first be removed.

```cpp
struct Sales_data {
    std::string bookNo;
    unsigned units_sold;
    double revenue;
}

Sales_data item = { "978-0590353403", 25, 15.99 };
```

# Exercise Section 7.5.6

## Exercise 7.53

### Question

Define your own version of `Debug`

### Answer

```cpp
class Debug {
public:
    constexpr Debug(bool b = true) : Debug(b, b, b) {
    }

    constexpr Debug(bool h, bool i, bool o) : hw(h), io(i), other(o) {
    }

    constexpr bool any() { return hw || io || other; }

    void set_io(bool b) { io = b; }
    void set_hw(bool b) { hw = b; }
    void set_other(bool b) { other = b; }

private:
    bool hw, io, other;
};
```

## Exercise 7.54

### Question

Should the members of `Debug` that begin with `set_` be declared as `constexpr`? If not, why not?

### Answer

**At the time of answering this question:**

The members beginning with `set_` cannot be marked as `constexpr` because `constexpr` functions are implicitly `const` and thus cannot mutate state.

**C++14 note**: `constexpr void` functions became available in C++ 14

## Exercise 7.55

### Question

Is the `Data` class from § 7.5.5 a literal class? If not, why not? If so, explain why it is literal

### Answer

The `Data` class is not a literal class because it uses `std::string`. To make it a literal type, `std::string` would need to be replaced with `const char *`

# Exercise Section 7.6

## Exercise 7.56

### Question

What is a `static` class member? What are the advantages of `static` members? How do they differ from ordinary members?

### Answer

A `static` class member is a member that is associated with the class itself rather than instances of the class. An advantage of using `static` members is that they do not form part of the memory requirements of a class and they allow the developer to reuse a single value throughout all instances of the class, such as an `interestRate` member.

## Exercise 7.57

### Question
Write your own version of the `Account` class

### Answer

```cpp
class Account {
public:
    void calculate() { amount += amount * interestRate; }
    static double rate() { return interestRate; }
    static void rate(double);
private:
    std::string owner;
    double amount;
    static double interestRate;
    static double initRate();
};
```

## Exercise 7.58

### Question

Which, if any, of the following `static` data member declarations and definitions are errors? Explain why.

```cpp
class Example {
public:
    static double rate = 6.5;
    static const int vecSize = 20;
    static std::vector<double> vec(vecSize);
};

double Example::rate;
std::vector<double> Example::vec(vecSize);
```

### Answer

`static` members are not part of the member instance and aren't initialized when creating instances of a class. In-class initializers may only be used where the initializer is an integral `const` or a literal `constexpr`.

```cpp
class Example {
public:
    constexpr static double rate = 6.5;
    static const int vecSize = 20;
    static std::vector<double> vec;
};

std::vector<double> Example::vec(vecSize);
```