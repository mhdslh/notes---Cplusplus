# notes---C++

Object-oriented programming:

1- For contructors that **can** be called with one argument, we must use explicit to avoid implicit convert types. If a class has a constructor which can be called with a single argument, then this constructor becomes a **conversion constructor** because such a constructor allows conversion of the single argument to the class being constructed. To avoid such implicit conversions as these may lead to unexpected results, we can make the constructor explicit.

2- For every class, compiler automatically creates a default **assignment operator** (operator=) and **copy constructor** (a constructor which can be called with an argument of the same class type).

3- **Conversion Operator** can be used to convert a class type to another type ([reference](https://en.cppreference.com/w/cpp/language/cast_operator)).

4- [Casting in C++](https://stackoverflow.com/questions/28002/regular-cast-vs-static-cast-vs-dynamic-cast): static cast performs conversion between compatible types for example to reverse an implicit conversion. It is similar to the C-style cast, but is more restrictive. Static cast performs no runtime checks. Dynamic cast is only used to convert object pointers or object references into other pointer or reference types in the inheritance hierarchy. Dynamic cast has runtime type checking.

5- In C++ access control works on per-class basis, not on per-object basis, i.e., objects of the same class have access to each other's private data. This is helpful when implementing copy constructor and assignment operator.

Next two points explain construction and destruction of data members:

6- The default constructor for every member is called before execution reaches the first line in the constructor, unless you explicitly specify a constructor using an initializer list, in which case that constructor is called instead.

7- When an object is cleaned up in C++, first the destructor for the class is called, and then the destructors for all the fields of the class. (If there's inheritance, the base class is then destroyed by recursively following this same procedure.) The destructor code that we write as part of the class implementation is just custom cleanup code that we'd like to do **in addition** to the normal cleanup code for individual data members. In fact, our destructor won't normally do anything to destroy objects contained within the object; what it typically does is destroy objects that are remotely owned.

-----
- "int" and "const int&" can not be used as lvalue, but "int&" can.
- copy elision
