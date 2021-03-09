# cpp-pointers-tutorial
> A simple tutorial on how the pointers work

```
             tab + 0   tab + 1
   Address  00E5F810  00E5F814  00E5F2D0  00E5F2D4  00E5F768  00E5F76C
     Value  00E5F2D0  00E5F768         1         2         3         4
                 [0]       [1]    [0][0]    [0][1]    [1][0]    [1][1]

tab = 00E5F810
tab + 1 = 00E5F814
*(tab + 1) = tab[1] = 00E5F768
*(*(tab + 1) + 1) = tab[1][1] = 4
```


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
    std::cout << "*(*(tab + " << row << ") + " << column << ") = tab[" << row << "][" << column << "] = " << *(*(table + row) + column) << std::endl;

    std::cout << std::endl;
}
```
