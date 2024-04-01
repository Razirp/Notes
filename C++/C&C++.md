# C/C++

## 目录

[TOC]

## 编译过程

C/C++程序的编译过程是一个将源代码转换成可执行程序的多步骤过程。这个过程通常包括以下几个主要阶段：

### 1. 预处理（Preprocessing）

在这个阶段，预处理器（preprocessor）读取源代码文件（通常是`.c`或`.cpp`文件），并对其中的预处理指令进行处理。这些指令以井号（`#`）开头，例如`#include`、`#define`、`#if`等。预处理器执行的操作包括：

- 包含头文件：将`#include`指令指定的文件内容插入到当前位置。
- 宏替换：将宏定义（如`#define`定义的常量和函数）替换到它们在源代码中出现的每个地方。
- 条件编译：根据`#if`、`#ifdef`、`#ifndef`等指令决定是否编译特定的代码块。
- 移除注释：删除源代码中的注释部分。

预处理器完成这些任务后，生成一个预处理后的源文件，通常没有扩展名，或者扩展名为`.i`（对于C）或`.ii`（对于C++）。

### 2. 编译（Compilation）

编译阶段是将预处理后的源代码转换成汇编代码或机器代码。编译器（如GCC、Clang、MSVC等）执行以下步骤：

- 词法分析（Lexical Analysis）：编译器将预处理后的源代码分解成一系列的标记（tokens），如关键字、标识符、常量、运算符等。
- 语法分析（Syntax Analysis）：编译器根据C/C++的语法规则检查标记的结构，并构建一棵抽象语法树（Abstract Syntax Tree, AST）。
- 语义分析（Semantic Analysis）：编译器检查AST中的语义一致性，如类型检查、变量声明、函数调用等。
- 生成中间代码：编译器将AST转换成中间代码，如三地址代码（3AC）或汇编指令。
- 优化（Optimization）：编译器对中间代码进行优化，以提高代码的执行效率和减少资源消耗。

编译阶段的输出是一个或多个目标文件（object files），通常具有`.o`或`.obj`扩展名，包含了汇编代码或机器代码。

### 3. 链接（Linking）

链接阶段是将一个或多个目标文件与库文件（libraries）合并，生成最终的可执行文件。链接器（linker）执行以下任务：

- 地址和空间分配（Address and Space Allocation）：为代码、数据分配内存空间。
- 符号解析（Symbol Resolution）：解析目标文件和库文件中的符号（如函数和变量名），确保所有引用的符号都能正确匹配。
- 重定位（Relocation）：调整代码和数据的地址，确保它们指向正确的内存位置。
- 输出可执行文件：生成最终的可执行文件，通常是`.exe`（在Windows上）或无扩展名的二进制文件（在Unix-like系统上）。

### 4. 运行（Execution）

用户运行可执行文件，操作系统加载它到内存中，并开始执行程序的入口点（通常是`main`函数）。

## 异常处理

### try-catch

在C++中，`try`和`catch`是用于异常处理的标准机制，它们允许程序在遇到错误或异常条件时优雅地恢复。这种机制允许程序捕获和处理可能在执行过程中抛出的异常，而不是让程序崩溃或产生未定义行为。
下面是`try`和`catch`的基本语法：

```cpp
try {
    // 可能抛出异常的代码
} catch (ExceptionType1 e) {
    // 处理特定类型异常的代码
} catch (ExceptionType2 e) {
    // 处理另一种类型异常的代码
} catch (...) {
    // 处理所有其他类型的异常的代码
}
```
在这个例子中，`try`块包含我们希望监控异常的代码。如果`try`块中的代码抛出了异常，控制流将转移到相应的`catch`块。
`catch`块可以有一个或多个，每个`catch`块可以捕获特定类型或所有类型的异常。如果异常的类型与`catch`块的类型匹配，则该`catch`块将被执行。如果没有任何`catch`块与异常类型匹配，程序将抛出一个未捕获的异常，这可能导致程序终止。
`catch`块中的代码用于处理异常，通常会执行一些清理工作，记录错误信息，或者尝试从错误中恢复。这可以防止程序因为未处理的异常而崩溃。
下面是一个具体的例子，演示如何使用`try`和`catch`来处理可能发生的异常：
```cpp
#include <iostream>
#include <vector>
#include <stdexcept>
int main() {
    std::vector<int> vec;
    try {
        // 尝试访问不存在的元素，将抛出out_of_range异常
        int value = vec.at(0);
    } catch (const std::out_of_range& e) {
        // 处理out_of_range异常
        std::cerr << "Error: " << e.what() << '\n';
        return 1;
    }
    // 其他代码...
    return 0;
}
```
在这个例子中，我们尝试访问一个空向量的第一个元素，这将抛出一个`std::out_of_range`异常。`catch`块捕获了这个异常，并打印出错误信息，然后返回1以指示程序发生了错误。
总的来说，`try`和`catch`提供了一种在C++中处理异常的机制，它允许程序在遇到错误时保持稳定，并提供一种恢复或报告错误的方式。这是C++异常处理的核心部分，是编写健壮和可靠程序的重要工具。

### 异常类型

在C++中，标准库定义了一系列的标准异常类型，用于表示不同类型的错误情况。这些异常类型都是从`std::exception`这个基类派生出来的。以下是一些常见的标准异常类型：

1. `std::exception`：所有标准异常的基类，提供了一个虚函数`what()`，用于返回异常的描述信息。
2. `std::bad_alloc`：当内存分配失败时抛出，通常是因为内存不足。
3. `std::bad_cast`：当类型转换操作（`dynamic_cast`）失败时抛出。
4. `std::bad_typeid`：当`typeid`操作失败时抛出。
5. `std::logic_error`：表示逻辑错误的异常类，如算法逻辑被违反。
   - `std::invalid_argument`：当一个函数接收到无效参数时抛出。
   - `std::out_of_range`：当一个操作超出了其有效范围时抛出。
   - `std::length_error`：当一个操作因为长度问题失败时抛出。
   - 等等。
6. `std::runtime_error`：表示运行时错误的异常类，如资源不足或文件操作失败。
   - `std::range_error`：当一个操作的范围不正确时抛出。
   - `std::overflow_error`：当数值运算结果超出表示范围时抛出。
   - `std::underflow_error`：当数值运算结果小于表示范围的下限时抛出。
   - 等等。

要处理默认的异常，你可以使用一个不带任何异常类型的`catch`子句，它会捕获所有类型的异常。这通常用于处理那些未被先前的`catch`子句捕获的异常，或者作为最后的“兜底”处理程序。
```cpp
try {
    // 可能抛出异常的代码
} catch (const std::exception& e) {
    // 处理所有从std::exception派生的异常
    std::cerr << "Standard exception: " << e.what() << '\n';
} catch (...) {
    // 处理所有其他类型的异常
    std::cerr << "Unknown exception caught.\n";
}
```
在这个例子中，第一个`catch`子句会捕获所有从`std::exception`派生的异常，而第二个`catch`子句会捕获所有其他类型的异常。通常，你应该尽可能具体地捕获异常类型，这样可以提供更详细的错误处理和更清晰的错误信息。

处理异常时，应该遵循以下最佳实践：
- 尽可能捕获和处理特定的异常类型。
- 使用`catch`子句的顺序很重要，具体的异常类型应该放在前面，通用的异常类型放在后面。
- 在`catch`子句中，提供有用的错误信息，并考虑清理资源和释放已分配的内存。
- 避免在`catch`子句中仅仅打印异常信息而不进行任何恢复操作，这可能会导致程序处于不稳定状态。

### 抛出异常

在C++中，抛出异常使用`throw`关键字。你可以抛出任何类型的异常，包括标准异常类（如`std::runtime_error`、`std::logic_error`等）的对象，也可以抛出自定义异常类的对象，或者是任何具有有效`what()`成员函数的对象。下面是一个抛出异常的基本示例：

```cpp
#include <iostream>
#include <stdexcept>

void functionThatThrows() {
    // 抛出一个运行时异常
    throw std::runtime_error("An error occurred in the function.");
}

int main() {
    try {
        functionThatThrows();
    } catch (const std::exception& e) {
        // 捕获并处理异常
        std::cerr << "Exception caught: " << e.what() << '\n';
    }
    return 0;
}
```

在这个例子中，`functionThatThrows`函数抛出了一个`std::runtime_error`异常。在`main`函数中，我们使用`try-catch`块来捕获并处理这个异常。

你还可以抛出自定义的异常类。首先，你需要定义一个继承自`std::exception`的类，然后创建这个类的实例并抛出：

```cpp
#include <iostream>
#include <stdexcept>

class MyCustomException : public std::exception {
public:
    MyCustomException(const std::string& message)
        : message_(message) {}

    virtual const char* what() const throw() {
        return message_.c_str();
    }

private:
    std::string message_;
};

void functionThatThrowsCustomException() {
    // 抛出一个自定义异常
    throw MyCustomException("A custom error occurred.");
}

int main() {
    try {
        functionThatThrowsCustomException();
    } catch (const std::exception& e) {
        // 捕获并处理异常
        std::cerr << "Custom exception caught: " << e.what() << '\n';
    }
    return 0;
}
```

在这个例子中，我们定义了一个名为`MyCustomException`的自定义异常类，它包含一个字符串消息，并重写了`what()`函数以返回这个字符串。然后，在`functionThatThrowsCustomException`函数中，我们创建了一个`MyCustomException`对象并抛出。

请注意，抛出异常是一个相对昂贵的操作，因为它涉及到栈展开（unwinding）和异常处理机制的执行。因此，在性能敏感的应用中，应该谨慎使用异常，并且在可能的情况下，考虑使用其他错误处理机制。

#### 抛出异常的昂贵性

抛出异常被认为是一个相对昂贵的操作，主要是因为它涉及到几个性能开销较大的步骤。以下是抛出异常时涉及的主要开销：

1. **栈展开（Stack Unwinding）**:
   当异常被抛出时，程序必须搜索合适的`catch`块来处理这个异常。这个过程涉及到从抛出点开始，逐层向上遍历调用栈，直到找到一个匹配的`catch`块。在这个过程中，每个函数的局部变量和栈帧都需要被清理和销毁，这会导致额外的开销。

2. **复制构造函数和析构函数的调用**:
   在搜索`catch`块的过程中，异常对象可能会被复制。如果异常对象是一个大对象或者包含复杂的数据结构，复制构造函数的调用会带来额外的开销。此外，每次异常对象被复制或移动时，析构函数也会被调用，这同样会增加开销。

3. **类型转换和匹配**:
   当异常被抛出时，C++需要检查抛出的异常类型是否与`catch`块中声明的类型匹配。这可能涉及到动态类型转换（`dynamic_cast`），特别是在处理多态异常类层次结构时。这个过程需要时间，并且可能涉及到运行时类型信息（RTTI）的查询。

4. **性能不可预测**:
   由于异常处理涉及到栈展开和可能的多次复制，它可能导致程序的性能变得不可预测。在性能敏感的应用中，这可能是一个问题，特别是在需要保证响应时间或实时性的场景中。

5. **异常处理的禁用**:
   在某些情况下，异常处理可能会被禁用，例如在C++中使用`-fno-exceptions`编译器选项。在这种情况下，抛出异常会导致程序终止，因为编译器不会生成处理异常的代码。这可能会导致意外的行为和难以调试的问题。

由于上述原因，抛出异常可能会对程序的性能产生影响。因此，在性能敏感的应用中，开发者可能会选择其他更轻量级的错误处理机制，以避免异常处理带来的开销。然而，异常处理仍然是C++中一个强大和灵活的错误处理工具，它可以在适当的情况下使用，特别是在处理那些无法预料的错误时。

#### 其他轻量级的错误处理机制

在性能敏感的应用中，异常处理可能会因为其开销而不太适用。因此，开发者通常会寻求其他的、更轻量级的错误处理机制。以下是一些常见的替代方案：

1. **错误码（Error Codes）**:
   使用返回值或错误码来表示操作的成功或失败是一种传统的错误处理方式。例如，你可以返回一个整数值，其中0表示成功，非0值表示错误，并对应特定的错误类型。这种方式避免了异常处理的开销，但需要开发者管理错误码并与函数的语义保持一致。

2. **布尔值和错误信息（Booleans and Error Information）**:
   通过返回一个布尔值来表示操作是否成功，并使用输出参数或单独的函数来获取错误信息。这种方式同样避免了异常处理的开销，但可能导致代码变得更加复杂，因为需要额外处理错误信息的传递。

3. **断言（Assertions）**:
   使用断言来检查代码中的条件，如果条件不满足，则程序终止。这种方式主要用于检查程序员认为不应该发生的条件，例如，输入参数的有效性。断言通常在开发和测试阶段启用，在生产环境中禁用。

4. **错误日志（Error Logging）**:
   记录错误信息而不是抛出异常。这可以通过使用日志库来完成，它可以记录错误发生的位置、时间和其他相关信息。错误日志对于调试和追踪问题非常有用，但不会中断程序的执行。

5. **错误处理函数（Error Handling Functions）**:
   使用特定的函数来检查操作是否成功，并处理错误。例如，许多系统调用和库函数会返回一个状态码，你可以使用这个状态码来决定下一步的操作。

6. **可选值（Optional Values）**:
   C++17引入了`std::optional`类型，它可以用来表示一个值可能存在，也可能不存在。这可以用于函数返回值，表示函数可能成功返回一个值，或者由于某些原因没有值可返。

7. **错误处理对象（Error Handling Objects）**:
   使用特殊的对象来封装错误信息。这些对象可以在检测到错误时被创建，并在需要时被检查。这种方式可以提供比简单的错误码更丰富的错误信息。

8. **事务和重试机制（Transactions and Retry Mechanisms）**:
   对于数据库操作或其他需要原子性的操作，可以使用事务来确保操作的完整性。如果操作失败，可以重试或回滚事务，而不是抛出异常。

选择哪种错误处理机制取决于多种因素，包括应用的特定需求、性能要求、代码的可读性和可维护性。在某些情况下，结合使用多种错误处理技术可能是最佳选择。例如，你可以在内部使用异常来处理错误，同时对外提供错误码或其他机制来通知调用者。

### std::exception_ptr

> - 跨线程传递异常
> - 重新抛出异常

`std::exception_ptr` 是 C++11 引入的一种类型，它提供了一种在异常安全的情况下传递异常对象的方法。`std::exception_ptr` 是一个指针类型，可以指向一个异常对象，或者为空（表示没有异常）。这个类型定义在 `<exception>` 头文件中，并且是 `std` 命名空间的一部分。

`std::exception_ptr` 的主要目的是解决在多线程环境下，特别是在使用 `std::future` 和 `std::promise` 时，跨线程传递异常的问题。由于异常不能直接跨线程传递，`std::exception_ptr` 允许你在一个线程中捕获异常，并将异常的指针存储起来，然后在另一个线程中重新抛出异常。

以下是 `std::exception_ptr` 的一些基本用法：

1. 创建 `std::exception_ptr` 对象：
   ```cpp
   std::exception_ptr ptr = std::current_exception();
   ```
   这里，`std::current_exception()` 函数捕获当前线程中的异常（如果有的话），并返回一个指向该异常的 `std::exception_ptr` 对象。如果没有异常发生，`ptr` 将为空。

2. 检查 `std::exception_ptr` 是否为空：
   ```cpp
   if (ptr) {
       // 有异常发生
   } else {
       // 没有异常发生
   }
   ```
   通过检查 `ptr` 是否为空，你可以判断是否有异常被捕获。

3. 重新抛出异常：
   ```cpp
   if (ptr) {
       std::rethrow_exception(ptr);
   }
   ```
   `std::rethrow_exception()` 函数接受一个 `std::exception_ptr` 对象，并重新抛出它所指向的异常。如果 `ptr` 为空，则会抛出 `std::bad_exception`。

4. 获取异常信息：
   ```cpp
   if (ptr) {
       try {
           std::rethrow_exception(ptr);
       } catch (const std::exception& e) {
           std::cerr << "Caught exception: " << e.what() << '\n';
       }
   }
   ```
   通过重新抛出异常并捕获它，你可以获取异常的类型和信息。

`std::exception_ptr` 在多线程编程中尤其有用，因为它允许你在不同的线程之间传递异常，而不需要担心异常的复制和移动问题。此外，它还可以用于异常的延迟处理，你可以在捕获异常后，根据需要在程序的任何地方重新抛出它。

需要注意的是，`std::exception_ptr` 并不存储异常的完整信息，它只是一个指向异常对象的指针。因此，为了获取异常的详细信息，你需要在有异常对象的上下文中重新抛出它。此外，`std::exception_ptr` 对象不能被复制，但可以被移动，这意味着你不能通过值传递 `std::exception_ptr`，而应该使用 `std::move` 来转移异常指针的所有权。

## 传递引用

`std::ref`和直接传递引用在C++中都是用来传递变量的引用的方式，但它们之间存在一些关键的区别：

### 使用`std::ref`传递引用

1. **封装**：`std::ref`创建了一个引用包装器，它封装了一个引用，并提供了与引用类似的接口。这意味着`std::ref`返回的是一个对象，而不仅仅是一个引用。
2. **可拷贝**：`std::ref`返回的引用包装器是可以拷贝的。每次拷贝都会创建一个新的引用包装器，它们都引用同一个原始对象。这在某些情况下非常有用，比如当你需要将引用传递给不接受非常量引用的函数或算法时。
3. **线程安全**：`std::ref`创建的引用包装器是线程安全的，可以用于多线程环境中的参数传递，而普通的引用在跨线程传递时需要额外的注意。
4. **灵活性**：`std::ref`可以用于创建引用的临时对象，这在某些情况下提供了更多的灵活性，例如在lambda表达式中使用。

### 直接传递引用

1. **直接性**：直接传递引用是最直接的方式，它不需要任何额外的封装。你只需要在函数参数中使用`&`来获取变量的引用。
2. **不可拷贝**：直接传递的引用不能被拷贝。尝试拷贝一个引用将导致编译错误，因为拷贝引用没有意义，它仍然是对同一对象的引用。
3. **线程安全**：直接传递的引用在跨线程时需要小心处理，因为不同线程对同一对象的访问可能需要同步机制来避免竞态条件。
4. **限制性**：直接传递的引用在某些情况下受到限制，比如不能传递给不接受非常量引用的函数或算法。

### 示例

以下是一个展示`std::ref`和直接传递引用区别的示例：

```cpp
#include <iostream>
#include <functional>

void modifyByReference(int& value) {
    value += 10;
}

void modifyByRefWrapper(int& value) {
    value += 20;
}

int main() {
    int x = 0;

    // 直接传递引用
    modifyByReference(x);
    std::cout << "Direct: " << x << std::endl; // 输出：10

    // 使用std::ref传递引用
    std::ref(x) = 30;
    modifyByRefWrapper(std::ref(x));
    std::cout << "Ref Wrapper: " << x << std::endl; // 输出：50

    return 0;
}
```

在这个示例中，`modifyByReference`函数直接接受一个引用参数，而`modifyByRefWrapper`函数接受一个`std::ref`包装器。在`main`函数中，我们首先直接传递`x`的引用，然后使用`std::ref`来修改`x`的值，并再次传递`std::ref`包装器给`modifyByRefWrapper`函数。

### 总结

`std::ref`提供了一种封装引用的方式，使得引用可以被拷贝和在线程间安全传递。直接传递引用则更直接、简单，但在某些情况下可能受到限制。在多线程编程和需要拷贝引用的场景中，`std::ref`是一个有用的工具。在其他情况下，直接传递引用可能更为合适。开发者应根据具体情况选择最合适的方法。

## 同步原语

> ### 临界区
>
> "临界区"（Critical Section）是并发编程中的一个基本概念，指的是访问共享资源的代码区域。在多线程环境中，确保临界区内的代码在同一时间只能被一个线程执行是非常重要的，因为多个线程同时访问和修改共享资源可能会导致数据不一致、竞态条件和不可预测的行为。
>
> 临界区的主要特征和目的包括：
>
> 1. **互斥访问**：确保同一时间只有一个线程可以执行临界区内的代码，防止多个线程同时读写共享资源。
>
> 2. **保护共享资源**：临界区通常用于保护那些不允许并发访问的共享资源，如全局变量、静态成员变量、共享文件或数据库等。
>
> 3. **避免竞态条件**：通过互斥机制，临界区可以避免竞态条件的发生，确保程序的正确性和一致性。
>
> 4. **同步控制**：临界区的实现通常涉及同步原语，如互斥锁（mutexes）、信号量（semaphores）、条件变量（condition variables）等，以控制对共享资源的访问。
>
> 下面是一个使用互斥锁（mutex）来保护临界区的C++代码示例：
>
> ```cpp
> #include <iostream>
> #include <mutex>
> #include <thread>
> 
> std::mutex mtx; // 用于保护临界区的互斥锁
> int sharedResource = 0; // 共享资源
> 
> void increment() {
>     std::lock_guard<std::mutex> guard(mtx); // 进入临界区
>     ++sharedResource;
> }
> 
> int main() {
>     std::thread t1(increment);
>     std::thread t2(increment);
>     t1.join();
>     t2.join();
>     std::cout << "Shared resource value: " << sharedResource << std::endl; // 输出应该是2
>     return 0;
> }
> ```
>
> 在这个例子中，`increment` 函数是一个临界区，因为它访问和修改了共享资源`sharedResource`。我们使用`std::lock_guard`来自动管理互斥锁`mtx`，确保在`increment`函数执行期间，其他线程不能同时进入临界区。当`increment` 函数执行完毕，`lock_guard`的析构函数会自动释放互斥锁，允许其他线程进入临界区。
>
> 通过这种方式，我们可以确保即使在多线程环境中，对共享资源的访问也是安全的，从而避免了竞态条件和其他并发问题。

在C/C++中，同步原语是用于控制并发访问共享资源的机制，以防止竞态条件和数据不一致。以下是一些常用的同步原语，以及它们的使用方式和代码示例。

### 1. 互斥锁（Mutexes）

互斥量（Mutual Exclusion）是一种同步对象，用于保护共享资源不被多个线程同时访问。当一个线程获取互斥量后，其他线程必须等待直到互斥量被释放。

- **用法**：在访问共享资源之前，线程尝试获取互斥量。如果互斥量已被占用，线程将阻塞，直到互斥量可用。
- **目的**：确保一次只有一个线程可以访问共享资源。

```cpp
#include <mutex>

std::mutex mtx;

void foo() {
    mtx.lock();
    // 临界区代码，访问共享资源
    mtx.unlock();
}
```

### 2. 条件变量（Condition Variables）

条件变量是一种同步机制，用于在特定条件下阻塞和唤醒线程。它通常与互斥量一起使用，以实现更复杂的同步逻辑。

- **用法**：线程在检查条件不满足时，会释放互斥量并等待条件变量。当条件变为满足时，线程被唤醒并重新尝试获取互斥量。
- **目的**：在特定条件成立时通知线程继续执行。

```cpp
#include <mutex>
#include <condition_variable>

std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void waiting_thread() {
    std::unique_lock<std::mutex> lock(mtx);
    while (!ready) {
        cv.wait(lock);
    }
    // 临界区代码
}

void notifying_thread() {
    std::unique_lock<std::mutex> lock(mtx);
    ready = true;
    cv.notify_all();
}
```

### 3. 读写锁（Read-Write Locks）

读写锁允许多个线程同时读取共享资源，但只允许一个线程写入。它是一种粒度更细的同步机制，提高了并发性能。

- **用法**：读取操作可以同时进行，写入操作则需要独占访问。线程在尝试写入时必须等待所有读取操作完成。
- **目的**：平衡读写操作的性能，确保数据的一致性。

```cpp
#include <shared_mutex>

std::shared_mutex rw_mtx;

void read() {
    std::shared_lock<std::shared_mutex> lock(rw_mtx);
    // 读取共享资源
}

void write() {
    std::unique_lock<std::shared_mutex> lock(rw_mtx);
    // 写入共享资源
}
```

### 4. 原子操作（Atomic Operations）

原子操作确保操作在单个不可分割的步骤中执行，无需担心其他线程的干扰。

```cpp
#include <atomic>

std::atomic<int> counter(0);

void increment() {
    ++counter; // 自增操作是原子的
}
```

### 5. 线程本地存储（Thread Local Storage）

线程本地存储为每个线程提供独立的数据副本，避免共享数据的并发问题。

```cpp
#include <thread>

thread_local int tls_counter = 0;

void increment_tls() {
    ++tls_counter; // 每个线程都有自己的tls_counter副本
}
```

### 6. 信号量（Semaphores）

信号量是一种计数器，用于控制多个线程对共享资源的访问。它允许多个线程同时访问资源，但数量受限。

- **用法**：线程在访问资源前尝试获取信号量（即计数器减一）。当信号量的计数为零时，线程阻塞。当线程释放资源时，信号量增加（即计数器加一），可能唤醒等待的线程。
- **目的**：限制对共享资源的并发访问数量。

```cpp
#include <semaphore.h>

std::semaphore sem(1);

void use_resource() {
    sem.acquire();
    // 使用资源
    sem.release();
}
```

### 7. 屏障（Barriers）

屏障是一种同步原语，用于确保一组线程在所有线程都达到某个执行点之后才能继续执行。

- **用法**：所有线程在屏障处等待，直到最后一个线程到达屏障。当所有线程都到达屏障时，它们才能一起继续执行。
- **目的**：同步一组线程的执行，确保它们在继续执行前都达到了某个状态。

```cpp
#include <tbb/parallel>

tbb::barrier barrier(num_threads);

void worker() {
    // 执行工作
    barrier.wait(); // 等待所有线程到达屏障
    // 继续执行后续工作
}
```

### 8. 未来和承诺（Futures and Promises）

未来（Future）和承诺（Promise）是C++11中引入的并发编程工具，用于异步操作和结果传递。

- **用法**：一个线程启动异步操作并返回一个未来对象，其他线程可以通过这个未来对象来获取异步操作的结果。
- **目的**：在异步操作完成前，允许程序继续执行其他任务，并在操作完成后获取结果。

```cpp
#include <future>

std::promise<int> prom;
std::future<int> fut = prom.get_future();

void compute() {
    int result = ...;
    prom.set_value(result); // 设置结果
}

int main() {
    std::thread t(compute);
    fut.wait(); // 等待结果
    int result = fut.get(); // 获取结果
    t.join();
}
```

这些同步原语提供了不同的并发控制机制，适用于不同的场景。在设计并发程序时，选择合适的同步原语对于确保数据一致性和程序性能至关重要。

### 智能锁

在C++中，`std::lock_guard`、`std::unique_lock`和`std::shared_lock`是标准库提供的智能锁（smart lock）类，它们用于管理互斥量的生命周期，确保在异常情况下也能安全地释放互斥资源。以下是这些锁的用法和代码示例：

#### 1. `std::lock_guard`

`std::lock_guard`是一个简单的RAII（Resource Acquisition Is Initialization）风格的互斥锁，它在构造时自动加锁，在析构时自动解锁。`std::lock_guard`不能被转移（move）所有权。

**用法**：直接在需要互斥保护的作用域内创建`std::lock_guard`对象。

```cpp
#include <mutex>
#include <iostream>

std::mutex mtx;

void printThread(int id) {
    std::lock_guard<std::mutex> guard(mtx); // 在作用域内自动加锁和解锁
    std::cout << "Thread " << id << " is running." << std::endl;
}

int main() {
    std::thread t1(printThread, 1);
    std::thread t2(printThread, 2);
    t1.join();
    t2.join();
    return 0;
}
```

#### 2. `std::unique_lock`

`std::unique_lock`是一个更灵活的RAII风格的互斥锁，它允许转移（move）所有权，并且可以在锁定和解锁之间切换。

**用法**：在需要互斥保护的作用域内创建`std::unique_lock`对象，可以使用`lock()`、`unlock()`和`release()`方法来控制互斥量。

```cpp
#include <mutex>
#include <iostream>

std::mutex mtx;
bool ready = false;

void waitForSignal() {
    std::unique_lock<std::mutex> guard(mtx);
    while (!ready) {
        std::cout << "Waiting for signal..." << std::endl;
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }
    // 离开作用域时自动解锁
}

void sendSignal() {
    std::unique_lock<std::mutex> guard(mtx);
    ready = true;
    // 这里可以解锁并重新加锁，或者转移所有权
}

int main() {
    std::thread t1(waitForSignal);
    std::thread t2(sendSignal);
    t1.join();
    t2.join();
    return 0;
}
```

#### 3. `std::shared_lock`

`std::shared_lock`是C++14中引入的，用于读写锁（`std::shared_mutex`）的共享模式。它允许多个线程同时获取共享锁（读锁），但写锁（排他锁）是互斥的。

**用法**：在需要读取共享资源的作用域内创建`std::shared_lock`对象。

```cpp
#include <shared_mutex>
#include <iostream>

std::shared_mutex rw_mtx;
int shared_data = 0;

void readData() {
    std::shared_lock<std::shared_mutex> guard(rw_mtx); // 获取共享锁
    std::cout << "Reading shared data: " << shared_data << std::endl;
}

void writeData(int data) {
    std::unique_lock<std::shared_mutex> guard(rw_mtx); // 获取排他锁
    shared_data = data;
    std::cout << "Writing data: " << shared_data << std::endl;
}

int main() {
    std::thread t1(readData);
    std::thread t2(writeData, 42);
    t1.join();
    t2.join();
    return 0;
}
```

在这个例子中，`readData`函数使用`std::shared_lock`来读取`shared_data`，而`writeData`函数使用`std::unique_lock`来修改`shared_data`。注意，`std::shared_lock`和`std::unique_lock`都需要`std::shared_mutex`来配合使用。

#### 4. `std::scoped_lock`

`std::scoped_lock`是C++17中引入的，它允许你一次性锁定多个互斥量，确保它们在作用域结束时一起被释放。这有助于避免死锁，并简化了多锁管理的代码。

**用法**：创建`std::scoped_lock`对象时传递多个互斥量。

```c++
#include <mutex>
#include <thread>
#include <scoped_lock>

std::mutex m1, m2;

void lockBoth() {
    std::scoped_lock lock(m1, m2); // 一次性锁定m1和m2
    // 临界区代码
}

int main() {
    std::thread t1(lockBoth);
    std::thread t2(lockBoth);
    t1.join();
    t2.join();
    return 0;
}
```

#### 总结

`std::lock_guard`、`std::unique_lock`和`std::shared_lock`提供了不同级别的灵活性和控制能力，适用于不同的并发场景。`std::lock_guard`适用于简单的互斥保护，`std::unique_lock`适用于需要精细控制的场景，而`std::shared_lock`适用于读写锁的场景。使用这些智能锁可以简化代码，并确保资源的正确释放。

## 日期与时间 - `std::chrono`

`std::chrono`是C++11标准引入的一个命名空间，它提供了一系列用于日期和时间处理的类和函数。`std::chrono`的组件被设计为高效、精确，并且易于使用，它们可以用来测量时间间隔、点的持续时间以及执行时间点的比较。

### 主要组件

1. **时间点（Time Points）**：表示特定的时间点，通常与持续时间（duration）一起使用来表示另一个时间点。
   - `std::chrono::time_point`：表示时间点的基类，通常与`std::chrono::steady_clock`等时钟一起使用。

2. **持续时间（Durations）**：表示两个时间点之间的时间长度。
   - `std::chrono::duration`：表示固定时间段的模板类，可以表示秒、毫秒、微秒等。
   - `std::chrono::duration_cast`：用于将`duration`对象转换为其他类型，如将其转换为秒数。

3. **时钟（Clocks）**：提供当前时间点的访问和等待功能。
   - `std::chrono::steady_clock`：提供一个单调的、不会被系统调整的时间源。
   - `std::chrono::system_clock`：提供系统时间，可能受到系统时间更改的影响。

4. **时间函数（Time Functions）**：提供对当前时间点的访问和转换。
   - `std::chrono::now`：返回给定时钟的当前时间点。

5. **时间格式化和解析**：`std::chrono`还提供了一些用于格式化和解析时间点的辅助函数。

### 示例

以下是一个使用`std::chrono`来测量函数执行时间的简单示例：

```cpp
#include <iostream>
#include <chrono>

void someFunction() {
    // 函数执行的代码
    std::this_thread::sleep_for(std::chrono::milliseconds(500)); // 模拟耗时操作
}

int main() {
    // 开始时间点
    auto start = std::chrono::high_resolution_clock::now();
    
    someFunction(); // 调用函数并测量执行时间
    
    // 结束时间点
    auto end = std::chrono::high_resolution_clock::now();
    
    // 计算并输出执行时间
    std::chrono::duration<double, std::milli> elapsed = end - start;
    std::cout << "Function took " << elapsed.count() << " ms to execute." << std::endl;
    
    return 0;
}
```

在这个示例中，我们使用`std::chrono::high_resolution_clock`来获取函数执行前后的时间点，并计算它们之间的差值，即函数的执行时间。

### 总结

`std::chrono`为C++提供了一套全面的日期和时间处理工具，它使得时间测量、时间点的比较和持续时间的计算变得简单而直观。通过使用`std::chrono`，开发者可以编写出更精确、更可靠的时间相关的代码。

## 防止隐式类型转换 - `explicit`

`explicit` 关键字在C++中用于防止隐式类型转换。当你在单参数构造函数或者具有默认参数的多参数构造函数前面加上 `explicit` 关键字时，你告诉编译器这个构造函数只能用于直接初始化，而不能用于隐式转换或复制初始化。

这个关键字的主要目的是提高程序的类型安全，防止编译器自动进行可能不合预期的类型转换。

### 没有 `explicit` 的情况

```cpp
class MyClass {
public:
    MyClass(int value) { /* ... */ }
    // 没有explicit
};

void function(MyClass obj) { /* ... */ }

int main() {
    function(42);  // 隐式转换，因为构造函数没有被声明为explicit
    return 0;
}
```

在上面的代码中，`MyClass` 的构造函数没有被声明为 `explicit`，所以编译器可以隐式地将 `int` 类型的值 `42` 转换为 `MyClass` 类型的对象，然后传递给 `function` 函数。

### 使用 `explicit` 的情况

```cpp
class MyClass {
public:
    explicit MyClass(int value) { /* ... */ }
    // 现在使用了explicit
};

void function(MyClass obj) { /* ... */ }

int main() {
    function(42);  // 错误！不能隐式转换
    function(MyClass(42));  // 正确，直接初始化
    return 0;
}
```

在这个例子中，我们在 `MyClass` 的构造函数前加上了 `explicit` 关键字。现在，编译器不再允许隐式地将 `int` 类型的值转换为 `MyClass` 类型的对象。这意味着你只能通过直接初始化的方式来创建 `MyClass` 的实例，如 `function(MyClass(42));`。

### 为什么使用 `explicit`？

使用 `explicit` 关键字可以避免一些容易引起错误的隐式类型转换，使得代码的意图更加明确，减少因为类型转换导致的意外行为。这有助于提高代码的可读性和可维护性。

### 注意事项

- `explicit` 只能用于构造函数，并且只能用于禁止隐式转换，不能禁止显式转换。
- 如果类中有多个单参数构造函数，你需要将所有的单参数构造函数都声明为 `explicit`，以确保一致性。
- `explicit` 关键字对于重载函数和转换操作符也是有用的，可以防止不期望的隐式类型转换。

总之，`explicit` 关键字是C++中一个重要的特性，它帮助开发者控制类的实例化过程，避免隐式类型转换带来的问题。