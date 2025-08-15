The focus is now on leveraging a strong C programming foundation for the transition to C++ for high-performance computing.

***

# 14-Day C++ Bootcamp for GPU Programming

## Introduction: From C Expert to C++ Architect

Welcome! This 14-day study plan is designed for experienced C programmers transitioning to modern C++ in preparation for advanced GPU and CUDA programming. Your background as a C programmer gives you an intuitive understanding of memory layouts, pointer arithmetic, and low-level control—a massive head start.

This plan aims to make you not just a C++ user, but a systems architect who understands the trade-offs between performance and abstraction. Each day, we will bridge concepts from C, highlighting why the C++ way is more effective and safer for writing the host-side code that orchestrates large-scale parallel systems like GPUs.

---

## Week 1: The Paradigm Shift from C to C++

### Day 1: "C with Classes" - Encapsulation and Abstraction

*   **Learning Objective:** Understand how to transition from C `struct`s and function pointers to C++ `class`es. Grasp the importance of encapsulation.
*   **Core Concepts (The "Why"):** In C, data (`struct`) and the functions that operate on it are separate. This makes it difficult to maintain a consistent state for your data. A C++ `class` bundles data (member variables) and functions (member methods) into a single unit, enforcing **encapsulation**. This dramatically improves code safety and maintainability in complex systems. In GPU programming, host code manages numerous states (device pointers, streams, events), and classes are the best tool for managing this complexity safely.
*   **Practical Exercise:**
    1.  Implement a simple stack in C using a `struct` and a set of functions (`push`, `pop`, `is_empty`, etc.).
    2.  Refactor this into a C++ `Stack` class. Make the data members `private` and the functions `public` member methods.

### Day 2: Constructors, Destructors, and RAII

*   **Learning Objective:** Understand constructors and destructors for automating object lifecycle management. Master the RAII (Resource Acquisition Is Initialization) pattern.
*   **Core Concepts (The "Why"):** In C, you must manually call `free` after `malloc`, or `fclose` after `fopen`. Forgetting to do so causes resource leaks. In C++, the **RAII** pattern uses the constructor to acquire a resource and the destructor to release it. The destructor is called automatically when an object goes out of scope, making resource leaks nearly impossible. In CUDA programming, `cudaMalloc` and `cudaFree` must always be paired; RAII is the most robust C++ technique to guarantee this.
*   **Practical Exercise:**
    1.  Create a `FileWrapper` class that holds a file pointer as a member.
    2.  Implement the constructor to open a file (`fopen`) and the destructor to close it (`fclose`).
    3.  In `main`, create a `FileWrapper` object and observe how the file is automatically closed when the function ends, even if errors occur.

### Day 3: C++ Memory Management and `std::vector`

*   **Learning Objective:** Learn the `new` and `delete` operators and compare them to C's `malloc`/`realloc`/`free`. Master `std::vector`, the standard for dynamic arrays.
*   **Core Concepts (The "Why"):** C's `malloc` only allocates raw memory. C++'s `new` allocates memory *and* calls the object's constructor. However, the most important concept here is `std::vector`. It's a dynamic array that automatically manages its own memory, eliminating nearly all risks of manual memory management (e.g., buffer overflows, leaks). When preparing large datasets to send to the GPU, `std::vector` is far safer and more convenient than C-style arrays.
*   **Practical Exercise:**
    1.  Take a C implementation of a dynamic array (using `realloc` to grow) and replace it with `std::vector`.
    2.  Practice using `push_back`, `size`, `capacity`, index-based access (``), and the safe `at()` member function.

### Day 4: STL Containers and Iterators

*   **Learning Objective:** Explore other key STL (Standard Template Library) containers like `std::array` and `std::string`. Understand iterators as the standard way to traverse them.
*   **Core Concepts (The "Why"):** The STL is a collection of highly optimized and proven data structures and algorithms. `std::array` combines the performance of a C-style array with the convenience of a C++ container. `std::string` solves the pain of C-style `char*` manipulation. **Iterators** are a generalization of pointers; they are the "glue" that allows standard algorithms to work on any container type in a uniform way.
*   **Practical Exercise:**
    1.  Create a fixed-size array with `std::array` and compare its features to `std::vector`.
    2.  Manipulate strings using `std::string`, replacing C-style `strcpy` and `strcat` with the `+` operator and member functions.
    3.  Get iterators from a vector's `begin()` and `end()` methods and use them in a `for` loop to print all elements.

### Day 5: STL Algorithms

*   **Learning Objective:** Learn to replace manual loops with STL algorithms like `std::sort`, `std::find`, and `std::accumulate`.
*   **Core Concepts (The "Why"):** In C, you write loops for everything. The STL provides one-line, highly efficient implementations for common tasks like sorting, searching, and summing. These algorithms work with iterators, so they can be applied to any container. This makes code more readable, less error-prone, and often faster.
*   **Practical Exercise:**
    1.  Create an `std::vector<int>` and fill it with random numbers.
    2.  Use `std::sort` to sort the vector instead of writing your own sorting loop.
    3.  Use `std::find` to check if a specific value exists.
    4.  Use `std::accumulate` to calculate the sum of all elements.

### Day 6: Function Templates - The Start of Generic Programming

*   **Learning Objective:** Write generic functions that are not tied to a specific data type.
*   **Core Concepts (The "Why"):** In C, you need to write a `max` function for `int` and a separate one for `float`. With **function templates**, you write a single generic `max` function that works for any type. The compiler generates the type-specific code for you at compile time. This is a core C++ feature that provides abstraction with **zero runtime overhead**, which is critical for performance. The CUDA Thrust library is built entirely on this technology.
*   **Practical Exercise:**
    1.  Write a function template `template<typename T> T max(T a, T b)` that takes two values and returns the greater one.
    2.  Call this function with `int`, `float`, and `double` arguments to verify it works for all of them.

### Day 7: Class Templates

*   **Learning Objective:** Write generic classes.
*   **Core Concepts (The "Why"):** Just like function templates, **class templates** allow you to create classes that are not tied to a specific type. `std::vector` is the prime example: it's really a class template, `std::vector<T>`. You can have a `std::vector<int>`, a `std::vector<float>`, or a vector of any type you define. This enables the creation of extremely flexible and reusable data structures.
*   **Practical Exercise:**
    1.  Convert the `Stack` class from Day 1 into a `Stack<T>` class template to create a generic stack that can hold any data type.
    2.  Create and test both `Stack<int>` and `Stack<double>` objects.

---

## Week 2: Modern C++ for High Performance

### Day 8: Smart Pointers - Ownership and Resource Management

*   **Learning Objective:** Use `std::unique_ptr` and `std::shared_ptr` to perform dynamic memory management without memory leaks.
*   **Core Concepts (The "Why"):** Even C++'s `new`/`delete` requires manual management. **Smart pointers** apply the RAII pattern to pointers. `std::unique_ptr` represents unique ownership of a resource and automatically `delete`s the object when it goes out of scope. `std::shared_ptr` allows multiple pointers to share ownership, and deletes the object only when the last shared pointer is destroyed. This is the standard modern C++ way to manage dynamic memory, making ownership clear and preventing leaks.
*   **Practical Exercise:**
    1.  Create an object with `new` and store it in a `std::unique_ptr`. Verify that the memory is freed without you ever calling `delete`.
    2.  Create multiple `std::shared_ptr`s to a single object and observe how its internal reference count changes as pointers are created and destroyed.

### Day 9: Inheritance and Polymorphism (and its cost)

*   **Learning Objective:** Understand base and derived classes, and learn about runtime polymorphism through `virtual` functions.
*   **Core Concepts (The "Why"):** Inheritance enables code reuse, while polymorphism allows you to treat different objects through a common interface. This is powerful for flexible design, but `virtual` function calls have a small runtime overhead due to indirection through a "v-table". In the most performance-critical inner loops of HPC code, this overhead is sometimes avoided in favor of compile-time polymorphism using templates. Understanding this trade-off is key.
*   **Practical Exercise:**
    1.  Create an abstract base class `Shape` with a pure virtual function `draw()`.
    2.  Create `Circle` and `Square` classes that inherit from `Shape` and implement their own `draw()` method.
    3.  Store pointers to `Circle` and `Square` objects in a `std::vector<Shape*>` and loop through it, calling `draw()` on each object.

### Day 10: Modern C++ Conveniences: `auto` and Range-Based `for`

*   **Learning Objective:** Learn key features from C++11 and later that make code more concise and readable.
*   **Core Concepts (The "Why"):** The `auto` keyword lets the compiler deduce a variable's type, turning a verbose declaration like `std::vector<MyVeryLongTypeName>::iterator it =...` into `auto it =...`. The **range-based `for` loop** (`for (auto& element : my_vector)`) is the modern, safe way to iterate over a container without manually handling iterators. These features significantly improve coding speed and readability.
*   **Practical Exercise:**
    1.  Refactor your existing `for` loops and iterator declarations to use range-based `for` and `auto`.

### Day 11: Lambda Expressions

*   **Learning Objective:** Learn to create anonymous, inline functions (lambdas) on the fly.
*   **Core Concepts (The "Why"):** When you want to pass a custom operation to an STL algorithm, you used to have to write a separate, named function. A **lambda expression**, like ` (int a, int b) { return a < b; }`, lets you define a simple function right where you use it. This makes code much more compact and self-contained.
*   **Practical Exercise:**
    1.  When sorting an `std::vector`, pass a lambda expression as the third argument to `std::sort` to sort in descending order.
    2.  Use `std::for_each` with a lambda to double every element in a vector.

### Day 12: Move Semantics and Rvalue References

*   **Learning Objective:** Understand the basic concept of move semantics for avoiding unnecessary data copies and improving performance.
*   **Core Concepts (The "Why"):** When returning a large `std::vector` from a function, older C++ would perform an expensive copy of the entire vector. **Move semantics** allows the resources (e.g., the pointer to the allocated memory) of a temporary object to be "moved" to a new object without a copy. This provides a massive performance boost when dealing with large data structures. It's an essential optimization in HPC where you handle gigabytes of data.
*   **Practical Exercise:** (Focus on conceptual understanding)
    1.  Read about what a move constructor and move assignment operator are, and the role of the `std::move` function.
    2.  Write a function that returns a `std::vector` and understand that the compiler often applies this optimization automatically (as Return Value Optimization, RVO).

### Day 13: Basic Concurrency: `std::thread` and `std::mutex`

*   **Learning Objective:** Use the C++ standard library to create multiple threads and protect shared data with a `std::mutex`.
*   **Core Concepts (The "Why"):** GPU programming is inherently about massive parallelism. While CUDA has its own threading model for the device, you often use multiple threads on the host CPU to manage data preprocessing or asynchronous operations. `std::thread` and `std::mutex` provide the C++ fundamentals for parallel execution and preventing race conditions, helping you train your parallel-thinking mindset.
*   **Practical Exercise:**
    1.  Create two threads using `std::thread`, having each one print a message.
    2.  Create a global counter and write a program where two threads safely increment it a million times each using a `std::mutex` to protect access.

### Day 14: Synthesis and Next Steps

*   **Learning Objective:** Synthesize the concepts from the past 13 days into a small project and see how this knowledge connects directly to GPU programming.
*   **Core Concepts (The "Why"):** You are now ready to combine the low-level control of C with the high-level abstraction and safety of C++. These skills are the foundation for writing the powerful host code that will launch kernels, prepare data, and orchestrate the performance of your entire system. For example, CUDA's Thrust library directly leverages your knowledge of STL algorithms and templates, and the CUDA Runtime API can be safely wrapped in classes using RAII.
*   **Final Project:**
    1.  Write a program that reads a list of numbers from a file into an `std::vector`.
    2.  Create a class that processes this vector (e.g., calculates the mean and standard deviation).
    3.  Use templates to make this class work for `int`, `float`, and `double`.
    4.  Write the results to an output file.
    5.  Actively use `auto`, range-based `for`, and smart pointers throughout your code.

After this 14-day bootcamp, you will be well-equipped with the power and safety of modern C++ to begin your exploration of the GPU world. Good luck!