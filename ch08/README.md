# Exercise Section 8.1.2

## Exercise 8.1

### Question

Write a function that takes and returns an `istream&`. The function should read the stream until it hits end-of-file. The function should print what it reads to the standard output. Reset the stream so that it is valid before returning the stream

### Answer

Ordinarily, one would use the `while (is >> input)` syntax which would also terminate on bad bit or fail bit. However, this exercise explicitly asks the reader to implement a function that reads to EOF, hence the check for `!is.eof()`

```cpp
std::istream &read(std::istream &is) {
    std::string input;
    while (!is.eof()) {
        is >> input;
        std::cout << input << std::endl;;
    }

    std::cout << "End" << std::endl;
    is.clear();

    return is;
}
```

## Exercise 8.2

### Question

Test your function by calling it, passing `std::cin` as an argument

### Answer

Tested. Behaves as expected

## Exercise 8.3

### Question

What causes the following `while` to terinate?

```cpp
while (std::cin >> i) /* ... */
```

### Answer

The aforementioned while syntax will terminate upon one of the following three scenarios

0. bad bit is set on `std::istream::iostate`
0. eof bit is set on `std::istream::iostate`
0. fail bit is set on `std::istream::iostate`

# Exercise Section 8.2.1

## Exercise 8.4

### Question

Write a function to open a file for input and read its contents into a `std::vector<std::string>`, storing each line as a separate element in the vector.

### Amswer

```cpp
void read(const std::string &fileName) {
    std::ifstream is(fileName);
    std::vector<std::string> lines;

    std::string input_line;

    while (std::getline(is, input_line)) {
        lines.emplace_back(input_line);
    }

    for (auto line : lines) {
        std::cout << line << std::endl;
    }
}
```

## Exercise 8.5

### Question

Rewrite the previous program to store each word in a separate element.

### Answer

```cpp
void read(const std::string &fileName) {
    std::ifstream is(fileName);
    std::vector<std::string> words;

    std::string input_word;

    while (is >> input_word) {
        words.emplace_back(input_word);
    }

    for (auto line : words) {
        std::cout << line << std::endl;
    }
}
```

## Exercise 8.6

### Question

Rewrite the bookstore program from § 7.1.1 to read its transactions from a file. Pass the name of the file as an argument to `main` (§ 6.2.5)

### Answer

```cpp
int main(int argc, const char *argv[]) {
    const char *fileName = argv[1];
    std::ifstream file(fileName);
    Sales_data total;
    if (read(file, total)) {
        Sales_data trans;
        while (read(file, trans)) {
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

# Exercise Section 8.2.2

## Exercise 8.7

### Question

Revise the bookstore program from the previous section to write its output to a file. Pass the name of that file as a second argument to `main`

### Answer

```cpp
int main(int argc, const char *argv[]) {
    const char *inputFileName = argv[1];
    const char *outputFileName = argv[2];
    
    std::ifstream inputFile(inputFileName);
    std::ofstream outputFile(outputFileName);
    
    Sales_data total;
    if (read(inputFile, total)) {
        Sales_data trans;
        while (read(inputFile, trans)) {
            if (total.isbn() == trans.isbn()) {
                total = add(total, trans);
            } else {
                print(outputFile, total) << std::endl;
                total = trans;
            }
        }
        print(outputFile, total) << std::endl;
    } else {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

## Exercise 8.8

### Question

Revise the program from the previous exercise to append its output to its given file. Run the program on the same output file at least twice to ensure that the data are preserved.

### Answer

```cpp
int main(int argc, const char *argv[]) {
    const char *inputFileName = argv[1];
    const char *outputFileName = argv[2];
    
    std::ifstream inputFile(inputFileName);
    std::ofstream outputFile(outputFileName, std::ofstream::app);
    
    Sales_data total;
    if (read(inputFile, total)) {
        Sales_data trans;
        while (read(inputFile, trans)) {
            if (total.isbn() == trans.isbn()) {
                total = add(total, trans);
            } else {
                print(outputFile, total) << std::endl;
                total = trans;
            }
        }
        print(outputFile, total) << std::endl;
    } else {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

# Exercise Section 8.3.1

## Exercise 8.9

### Question

Use the function you wrote for the first exercise in § 8.1.2 to print the contents of an `istringstream` object

### Answer

```cpp
std::istream &read(std::istream &is) {
    std::string input;
    while (!is.eof()) {
        is >> input;
        std::cout << input << std::endl;;
    }

    std::cout << "End" << std::endl;
    is.clear();

    return is;
}

int main(int argc, const char *argv[]) {
    std::istringstream sstrm("Hello");
    read(sstrm);
}
```

## Exercise 8.10

### Question

Write a program to store each line from a file in a `std::vector<std::string>`. Now use an `std::istringstream` to read each element from the `std::vector` a word at a time.

### Answer

```cpp
int main(int argc, const char *argv[]) {
    std::ifstream ifs("input.txt");
    std::vector<std::string> lines;

    std::string line, word;
    while (std::getline(ifs, line)) {
        lines.emplace_back(line);
    }

    for (const auto &line : lines) {
        std::istringstream iss(line);

        while (iss >> word) {
            std::cout << word << std::endl;
        }
    }
}
```

## Exercise 8.11

### Question

The program in this section defined its `std::istringstream` object inside the outer `while` loop. What changes would you need to make if `record` were defined outside that loop? Rewrite the program, moving the definition of `record` outside the `while`, and see whether you thought of all the changes that are needed

### Answer

```cpp
void func() {
    std::string line, word;
    std::vector<PersonInfo> people;
    std::istringstream record;
    
    while (std::getline(std::cin, line)) {
        PersonInfo info;
        
        record.clear();
        record.str(line);
        record >> info.name;
        
        while (record >> word)
            info.nums.emplace_back(word);
        
        people.emplace_back(info);
    }
}
```

## Exercise 8.12

### Question

Why didn't we use in-class initializers in `PersonInfo`?

### Answer

The class uses types that have synthesized constructors. There is no need to use in-class initializers.

# Exercise Section 8.3.2

## Exercise 8.13

### Question

Rewrite the phone number program from this section to read from a named file rather than from `std::cin`

### Answer

```cpp
std::istream &func(std::istream &is) {
    std::string line, word;
    std::vector<PersonInfo> people;
    std::istringstream record;

    while (std::getline(is, line)) {
        PersonInfo info;

        record.clear();
        record.str(line);
        record >> info.name;

        while (record >> word)
            info.nums.emplace_back(word);

        people.emplace_back(info);
    }
    
    return is;
}

int main(int argc, const char *argv[]) {
    std::ifstream ifs("input.txt");
    func(ifs);
}
```

## Exercise 8.14

### Question

Why did we declare `entry` and `nums` as `const auto &`

### Answer

0. `const` because we don't change the value
0. `auto` to infer the type
0. `&` to capture as a reference - no copying involved