# cpp-pointers-tutorial
> A simple tutorial on how the pointers work - a two-dimensional array example

```
             tab + 0   tab + 1
   Address  00740368  0074036C  00740800  00740804  00740838  0074083C
     Value  00740800  00740838         1         2         3         4
                 [0]       [1]    [0][0]    [0][1]    [1][0]    [1][1]

tab = 00740368
tab + 1 = 0074036C
*(tab + 1) = tab[1] = 00740838
*(tab + 1) + 1 = 0074083C
*(*(tab + 1) + 1) = tab[1][1] = 4
&tab[1][1] = 0074083C
```

### Here's an explaination what's going on

There are three (3) one-dimensional tables. The second one owns the columns of the first row, the third one owns the columns of the second row. The first table is a table of pointers. The first element is a pointer to the first row, the latter is a pointer to the second row.

### Hey, what does `*`, `&`, `*&` and `**` do?

- [What are the differences between a pointer variable and a reference variable in C++?](https://stackoverflow.com/questions/57483/what-are-the-differences-between-a-pointer-variable-and-a-reference-variable-in)
- [Meaning of *& and **& in C++](https://stackoverflow.com/questions/5789806/meaning-of-and-in-c)
- https://en.cppreference.com/w/cpp/language/pointer
- https://en.cppreference.com/w/cpp/language/reference
- https://cdecl.org/

```cpp
#include <iostream>
#include <iomanip>
#include <sstream>

template<typename T>
void print(T **table, size_t rows, size_t columns);

template<typename T>
void printValue(T **table, size_t row, size_t column);

int main() {
    // * - get value from key: *key
    // & - get key from variable: &variable

    const int rows = 2;
    const int columns = 2;

    // Create new dynamic array
    int **table = new int*[rows];

    for (int i = 0; i < 2; i++) {
        table[i] = new int[columns];
    }

    // Provide some values
    table[0][0] = 1;
    table[0][1] = 2;
    table[1][0] = 3;
    table[1][1] = 4;

    // Display the table
    print(table, rows, columns);
    printValue(table, 1, 1);

    // Free the table
    for (int i = 0; i < 2; i++) {
        delete[] table[i];
    }

    delete[] table;

    // Return process exit code 0
    return 0;
}

template<typename T>
void print(T **table, size_t rows, size_t columns) {
    // tab + n
    {
        std::cout << std::setw(10) << "";

        int index = 0;
        for (int **key = table; key < table + rows; key++) {
            std::stringstream ss;
            ss << "tab + " << index++;

            std::cout << std::setw(10) << ss.str();
        }

        std::cout << std::endl;
    }

    // Keys
    {
        std::cout << std::setw(10) << "Address";

        for (int **key = table; key < table + rows; key++) {
            std::cout << std::setw(10) << key;
        }

        for (int **key = table; key < table + rows; key++) {
            for (int *secondKey = *key; secondKey < *key + columns; secondKey++) {
                std::cout << std::setw(10) << secondKey;
            }
        }

        std::cout << std::endl;
    }

    // Values
    {
        std::cout << std::setw(10) << "Value";

        for (int **key = table; key < table + rows; key++) {
            std::cout << std::setw(10) << *key;
        }

        for (int **key = table; key < table + rows; key++) {
            for (int *secondKey = *key; secondKey < *key + columns; secondKey++) {
                std::cout << std::setw(10) << *secondKey;
            }
        }

        std::cout << std::endl;
    }

    // [rows][columns]
    {
        std::cout << std::setw(10) << "";

        int index = 0;
        int secondIndex = 0;

        for (int **key = table; key < table + rows; key++) {
            std::stringstream ss;
            ss << "[" << index++ << "]";

            std::cout << std::setw(10) << ss.str();
        }

        for (index = 0; index < rows; index++) {
            for (secondIndex = 0; secondIndex < columns; secondIndex++) {
                std::stringstream ss;
                ss << "[" << index << "]" << "[" << secondIndex << "]";

                std::cout << std::setw(10) << ss.str();
            }
        }

        std::cout << std::endl;
    }

    std::cout << std::endl;
}

template<typename T>
void printValue(T **table, size_t row, size_t column) {
    std::cout << "tab = " << table << std::endl;
    std::cout << "tab + " << row << " = " << (table + row) << std::endl;
    std::cout << "*(tab + " << row << ") = tab[" << row << "] = " << *(table + row) << std::endl;
    std::cout << "*(tab + " << row << ") + " << column << " = " << *(table + row) + column << std::endl;
    std::cout << "*(*(tab + " << row << ") + " << column << ") = tab[" << row << "][" << column << "] = " << *(*(table + row) + column) << std::endl;
    std::cout << "&tab[" << row << "][" << column << "] = " << &table[row][column] << std::endl;

    std::cout << std::endl;
}
```
