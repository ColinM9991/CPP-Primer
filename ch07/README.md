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