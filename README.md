# Yibtags C++ Style guide

> [!WARNING]  
> This is subject to change. You may also suggest any changes along with an exmplenation on why youre change is for the better.

## Naming an form

### Variables

When naming a variable you should use snake case.

Example of naming a variable:
```cpp
int first_secound_third = 5;
```

Arrays should be indented per value, unsless the value itself is very short in charachter leanght.

Example of using arrays:
```cpp
std::array<int, 3> short_array = {
    5, 1, 2
};

std::array<int, 2> long_array = {
    number_1,
    number_2
};
```

When accessing a member variable you should use the this keyword to make it clear you are accessing a member variable.

Example of accessing a member variable:
```cpp
this->member = 5;
```

Unused varibles should be avoided but when they are necesery you should prefix the variable name with an underscore.

Example of naming a unused variable:
```cpp
int _unused = 5;
```

### Functions

Functions should be named in pascal case. If a function has no parameters it should look like this.

Example of naming a function with no arguments:
```cpp
void Run() {

}
```

If a function has any arguments they should be indented as such.

Example of naming a function with arguments:
```cpp
int AddTwoNumbers(
    const int& first,
    const int& secound
) {
    return first + secound;
}

int AddFive(
    const int& first
) {
    return AddTwoNumbers(first, 5);
}
```

### Structs

Structs should be named in pascal case. When naming structs you should use the convention as shown below to allow shortenings for pointers and such. The pointer value should be prefixed by an uppercase P. Templated structs should use the following syntax.

Example of naming a struct:
```cpp
struct _Data {
    int size;
    char* first;
} Data, *PData;

template<typename T>
struct _Templated {
    T value;
}

template<typename T>
using Templated = _Templated<T>;

template<typename T>
using PTemplated = _Templated<T>*;
```

### Classes

Classes should be named in pascal case.

Diffrent types of functions should be separated:
 - Constructors
 - Internal constructors (functions that initilize member variables)
  - Getters
  - Setters
  - Normal functions
  - ...

Diffrent types of variables should also be separated:
 - Dependencies
 - Normal variables
 - ...

They should aloso be listed by first in last out ordered as thats how c++ handles creation of destructions of variables.

Example of naming a class:
```cpp
#include <string>

class Object {
public:
    Object();

    int GetId();

    void DoAction(
        std::string action
    );
private:
    bool CreateInternal();
    void DestroyInternal();

    Dependency dependency;

    int id;
};
```

### Includes

Includes that are only used in the implementation files should stay there. Includes should be separeted by source. Includes should be separated in this order:
 - The standard library
 - Third party libraries
 - Project files
 - ...

Includes per separation should be ordered by leanght.

Example of includes:
```cpp
// STD library
#include <array>
#include <string>

// Third party libraries
#include <vulkan/vulkan.h>

// Project files
#include "buffer.h"
```

## Types

C types should be avioded, unless absolutly required. These include c-strings and c-arrays wich both have c++ counter parts that arent effected by the same problem.

Exaple of c++ alterniteves:
```cpp
#include <string>

const char* c_string = "Hello, world!";

std::string cpp_string = "Hello, world!";

#include <array>

int c_array[] = {
    5, 1, 2
};

std::array<int, 3> cpp_array = {
    5, 1, 2
};
```

Pointers should be avoided when passing arguments to a function. Refrences should be used instead. They should be made constant if they are only being read.

Example of function parameters:
```cpp
#include <iostream>

void Change(int& state) {
    state = 5;
}

void Read(const int& data) {
    std::cout << data << std::endl;
}
```

When you are in need of a data obstraction that doesent implement any methods you should use structs.

Example of unnecessary use of classes:
```cpp
class Data {
public:
    int size;
    char* first;
};

struct _Data {
    int size;
    char* first;
} Data, *PData;
```

## Errors

You should treat errors as values. Simple as that. This depends on the project but every project should have an error class or struct.

Example of a error type:
```cpp
typedef std::optional<std::string> Result;
```

When dealing with constructors that error, they should have a private member variable that acts as a return value.

Example of constructor errors:
```cpp
class Example {
public:
    Example() {
        if (!failed_function()) {
            this->result = "Failed to init ...!";
            return;
        }

        this->result = std::nullopt;
    }

    Result GetResult() const {
        return this->result;
    }
private:
    Result result;
};
```

## Inharitance

Instead of inharitance we use interfaces (wich can only implement methods) and dependency injection.

Example of dependency injection:
```cpp
class Device {
    // ...
};

class Renderer {
private:
    // Dependency
    Device device;
}
```

Example of using interfaces:
```cpp
class UIComponent {
public:
    virtual void Draw() = 0;
};

class UIButton : public UIComponent {
public:
    void Draw() override {};
};
```

## Comments

Comments should not be used unless necessary basic usage casses and examples belong in documentation files. You should only comment things that are generaly hard to understant(example: randome bitshifts or functions being called for seamingly no reason).