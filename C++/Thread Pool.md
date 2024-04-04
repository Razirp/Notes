# Thread Pool

> 线程池 - “池化”思想

- 一种常用的并发编程设计模式
- 管理线程的创建、执行和销毁
- 提高应用程序的性能和响应速度

## 核心思想

预先创建一定数量的线程，并把它们放入一个池中，等待执行任务。

当有新任务到来时，线程池会从空闲的线程中选择一个来执行这个任务，而不是每次都创建一个。

- 可以减少系统频繁创建和销毁线程的开销
- 提高系统资源的利用率
- 使得多线程程序设计更加简单、高效

## 工作原理

1. **初始化阶段：**在应用程序启动时，线程池会根据预定的参数创建一定数量的线程，并将它们置于**待命状态**
2. **任务提交阶段：**当有新任务到来时，任务会被提交到线程池中的**任务队列**
3. **任务执行阶段：**线程池中的空闲线程会从任务队列中取出任务并执行
4. **任务完成阶段：**任务执行完成后，线程不会立即销毁，而是返回线程池中继续等待下一个任务

## 优点

- **提高响应速度：**因为线程**预先创建**，故可以迅速响应外部请求，减少线程创建带来的延迟
- **降低资源消耗：**通过**重用**已创建的线程来执行任务，避免了频繁创建和销毁线程的开销，从而降低系统资源的消耗
- **提高线程的可管理性：**通过限制线程的数量，可以避免系统过载；因为线程过多时会降低系统性能
  - 过分调度问题：当线程数量超过了处理器核心数时，会导致一些线程在等待其他线程释放资源，而这些资源可能永远不会被释放，因为所有线程都在等待
    - 线程之间存在大量的等待和竞争，导致整体效率降低
  - 资源消耗增加：每个线程都会占用一定的系统资源，如CPU时间片、内存空间等
  - 上下文切换开销：操作系统调度线程时，需要保存当前线程的状态
  - 系统调度复杂度
  - 内存溢出风险
- **提供更丰富的功能：**通常提供任务调度、任务优先级、任务拒绝策略等丰富的功能，使得多线程程序设计更加灵活

## 组成成分

- **工作线程：**线程池中的线程，用于执行任务
- **任务队列：**存放等待执行的任务
- **线程工厂：**用于创建新线程
  - 可以根据需要定制线程的属性
- **拒绝策略：**当任务队列满且工作线程的数目达到最大值时，新提交的任务如何处理的策略

## 应用场景

服务器端编程、Web应用、大数据处理、数据库操作等，需要处理大量并发任务的场景。

如：

- 在Web服务器中，每个客户端请求可能需要创建一个新的线程来处理
  - 使用线程池可以有效管理这些线程，提高服务器的并发处理能力
- 日志落盘
  - 避免等待
- 异步操作
  - close(fd);

## Java 实现

Java中，ExecutorService是线程池的核心接口，通过实现这个接口，可以创建自己的线程池。

Java提供了几种不同的线程池实现：FixedThreadPool、CachedThreadPool、ScheduledThreadPool等。

## C/C++ 实现

### 线程的标识

大多数操作系统都提供了一种机制来获取当前线程的唯一标识符。例如，在POSIX线程（pthread）库中，可以使用`pthread_self()`函数来获取当前线程的线程ID。在Windows系统中，可以使用`GetCurrentThreadId()`函数来获取当前线程的ID。

```c
// POSIX线程（pthread）示例
#include <pthread.h>

pthread_t threadId = pthread_self();
// 使用threadId作为线程标识符

// Windows示例
DWORD threadId = GetCurrentThreadId();
// 使用threadId作为线程标识符
```

### 线程管理

`pthread_t`是POSIX线程（Portable Operating System Interface for uniX，POSIX）的一部分，用于在支持POSIX线程的操作系统中表示和控制线程。以下是使用`pthread_t`管理线程的详细步骤和概念：

#### 1. 包含头文件

在使用`pthread_t`之前，需要包含`<pthread.h>`头文件，它提供了创建和管理线程所需的所有函数和数据类型定义。

```c
#include <pthread.h>
```

#### 2. 创建线程

使用`pthread_create()`函数创建线程。这个函数接受四个参数：

- `pthread_t *thread`：指向`pthread_t`类型的指针，用于存储新创建线程的标识符。
- `const pthread_attr_t *attr`：指向`pthread_attr_t`结构的指针，用于指定新线程的属性。如果不需要设置特定属性，可以传递`NULL`。
- `void *(*start_routine)(void *)`：新线程的入口函数，必须是一个返回`void *`类型并接受`void *`类型参数的函数指针。
- `void *arg`：传递给线程入口函数的参数。

```c
void *threadFunction(void *arg) {
    // 线程要执行的任务
    return NULL;
}

int main() {
    pthread_t threadId;
    int result = pthread_create(&threadId, NULL, threadFunction, NULL);
    if (result != 0) {
        // 错误处理
    }
    // ...
    return 0;
}
```

#### 3. 等待线程结束

使用`pthread_join()`函数等待线程结束。这个函数同样接受两个参数：

- `pthread_t thread`：要等待的线程的标识符。
- `void **retval`：指向`void *`类型的指针的指针，用于存储线程的返回值。如果不需要线程返回值，可以传递`NULL`。

```c
void *status;
result = pthread_join(threadId, &status);
if (result != 0) {
    // 错误处理
}
```

#### 4. 线程分离

使用`pthread_detach()`函数可以将线程与`pthread_t`标识符分离，这样即使`pthread_t`对象被销毁，线程仍然会继续执行。分离后的线程在结束时会自动清理资源。

```c
result = pthread_detach(threadId);
if (result != 0) {
    // 错误处理
}
```

#### 5. 线程属性

可以通过`pthread_attr_t`结构设置线程的属性，如栈大小、调度策略等。创建`pthread_attr_t`对象后，可以使用一系列`pthread_attr_*`函数来设置属性。

```c
pthread_attr_t attr;
pthread_attr_init(&attr);
pthread_attr_setstacksize(&attr, stack_size); // 设置栈大小
// ...
result = pthread_create(&threadId, &attr, threadFunction, NULL);
```

#### 6. 错误处理

每个`pthread`函数都会返回一个整数状态码。如果函数调用失败，状态码非零，可以通过`strerror()`函数获取错误描述。

#### 7. 线程同步

`pthread`库提供了多种线程同步机制，包括互斥锁（`pthread_mutex_t`）、条件变量（`pthread_cond_t`）、读写锁（`pthread_rwlock_t`）等。

#### 8. 线程取消

`pthread`库支持线程取消。可以使用`pthread_cancel()`函数请求取消一个线程，但要注意线程取消的安全性和线程的协作取消。

#### 总结

使用`pthread_t`管理线程涉及创建线程、等待线程结束、分离线程、设置线程属性、错误处理和线程同步等步骤。这些函数和概念为多线程编程提供了底层的控制，使得开发者可以充分利用多核处理器的优势，提高程序的并发性和执行效率。然而，直接使用`pthread`库需要对线程的生命周期和同步有深入的理解，以避免常见的多线程问题，如死锁、竞态条件等。

### std::thread

`std::thread`是C++11标准引入的一个类，用于表示和控制线程。它定义在`<thread>`头文件中，提供了创建、管理和同步线程的接口。以下是`std::thread`的基本用法、语法和一些关键特性的详细介绍。

### 基本用法和语法

创建一个`std::thread`对象时，你需要提供一个可调用对象（如函数、函数指针、lambda表达式或重载了`operator()`的类的实例）作为线程的入口点。你还可以提供额外的参数传递给这个可调用对象。

```cpp
#include <thread>
#include <iostream>

void threadFunction(int id) {
    std::cout << "Hello from thread " << id << std::endl;
}

int main() {
    // 创建并启动一个线程
    std::thread t1(threadFunction, 1);

    // 等待线程完成
    t1.join();

    return 0;
}
```

### 关键特性

1. **构造函数**：`std::thread`的构造函数接受一个可调用对象和它的参数。构造函数执行时，线程立即启动并执行给定的可调用对象。

2. **`joinable()`方法**：此方法用于检查`std::thread`对象是否关联着一个活动的线程。如果对象可被`join`，则返回`true`。

3. **`join()`方法**：调用`join()`方法会阻塞当前线程，直到`std::thread`对象关联的线程完成执行。如果`std::thread`对象没有关联线程或线程已经完成，则`join()`立即返回。

4. **`detach()`方法**：`detach()`方法将线程与`std::thread`对象分离，允许线程独立于创建它的线程继续运行。分离后的线程在完成执行后会自动清理其资源。

5. **`get_id()`方法**：返回一个表示线程ID的对象，可以用来识别和比较线程。

6. **移动语义**：`std::thread`支持移动语义，不支持拷贝操作。这意味着你可以使用`std::move`来转移线程的所有权，但不能复制线程。

### 线程参数传递

线程函数可以通过多种方式接收参数：

- 直接传递值或指针。
- 使用`std::ref`来传递引用。
- 使用`std::cref`和`std::move`来传递可调用对象和它们捕获的变量。

```cpp
int value = 42;
std::thread t2([&value](){ std::cout << "Value: " << value << std::endl; }, std::ref(value));
```

### 线程分离

当线程不再需要时，可以调用`detach()`方法来分离线程，这样线程将继续在后台运行，而不需要`std::thread`对象的生命周期。

```cpp
std::thread t3(threadFunction, 2);
t3.detach(); // 分离线程
```

### 线程同步

除了`join()`方法外，`std::thread`还与其他同步原语（如互斥量、条件变量、future等）协同工作，以实现线程间的同步和通信。

### 异常

`std::thread`的构造函数和`join()`、`detach()`等方法都可以抛出异常。例如，如果线程无法创建，构造函数将抛出`std::system_error`。

### 总结

`std::thread`为C++提供了一个高级、易用且跨平台的线程管理接口。它简化了多线程编程的复杂性，使得并发编程更加容易实现和维护。通过合理使用`std::thread`，开发者可以有效地利用多核处理器的计算能力，提高程序的性能和响应性。

## 线程池参数配置方案

![img](https://p0.meituan.net/travelcube/23a44974ff68a08261fb675242b83648181953.png)