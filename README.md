# notes---C++

Object-oriented programming:

1- For contructors that **can** be called with one argument, we must use explicit to avoid implicit convert types. If a class has a constructor which can be called with a single argument, then this constructor becomes a **conversion constructor** because such a constructor allows conversion of the single argument to the class being constructed. To avoid such implicit conversions as these may lead to unexpected results, we can make the constructor explicit.

2- For every class, compiler automatically creates a default **assignment operator** (operator=) and **copy constructor** (a constructor which can be called with an argument of the same class type).

3- **Conversion Operator** can be used to convert a class type to another type ([reference](https://en.cppreference.com/w/cpp/language/cast_operator)).

4- [Casting in C++](https://stackoverflow.com/questions/28002/regular-cast-vs-static-cast-vs-dynamic-cast): static cast performs conversion between compatible types for example to reverse an implicit conversion. It is similar to the C-style cast, but is more restrictive. Static cast performs no runtime checks. Dynamic cast is only used to convert object pointers or object references into other pointer or reference types in the inheritance hierarchy. Dynamic cast has runtime type checking.

5- In C++ access control works on per-class basis, not on per-object basis, i.e., objects of the same class have access to each other's private data. This is helpful when implementing copy constructor and assignment operator.

6- Only the member functions or the friend functions are allowed to access the private data members of a class, i.e., class members declared as private are not allowed to be accessed directly by any object or function outside the class. Protected access modifier is similar to that of private access modifiers; the difference is that the class member declared as Protected are inaccessible outside the class but they can be accessed by any subclass (derived class) of that class.

Next two points explain construction and destruction of data members:

7- The default constructor for every member is called before execution reaches the first line in the constructor, unless you explicitly specify a constructor using an initializer list, in which case that constructor is called instead.

8- When an object is cleaned up in C++, first the destructor for the class is called, and then the destructors for all the fields of the class. (If there's inheritance, the base class is then destroyed by recursively following this same procedure.) The destructor code that we write as part of the class implementation is just custom cleanup code that we'd like to do **in addition** to the normal cleanup code for individual data members. In fact, our destructor won't normally do anything to destroy objects contained within the object; what it typically does is destroy objects that are remotely owned.

Inheritance and Polymorphism:

9-a) For polymorphism, "virtual" **must** be used for the base class function declaration (for the first appearance). In the derived class the function is virtual by way of having the same type as the base class function. Optionally, "virtual" keyword in the derived class ensures that the function is still virtual in the further derived classes. 

9-b) "override" ensures that the function is overriding a virtual function from a base class. A compile-time error is generated if this is not true. Without "override", when the function is ill-formed, the closest virtual function in the inheritance hierarchy is called (no compilation error).

9-c) "final" ensures that a virtual memebr function can not be overriden by derived classes. Similarly, "final", in class definition, specifies that no classes can be dervied from this class.


Data Structures:
- [**std::set**](https://cplusplus.com/reference/set/set/) stores unique elements following a specific order. "set" containers are generally slower than "std::unordered_set" containers to access individual elements by their key. It is typically implemented as binary search trees.
- [**std::unordered_set**](https://cplusplus.com/reference/unordered_set/unordered_set/) (hash set) stores unique elements in no particular order. Elements in unordered_set containers are organized into buckets depending on their hash values to allow for fast access to individual elements directly by their values.
- [**std::map**](https://cplusplus.com/reference/map/map/) stores elements formed by a combination of a key value and a mapped value. The elements in a map are always sorted by its key following a specific order. Maps are typically implemented as binary search trees.
- [**std::unordered_map**](https://cplusplus.com/reference/unordered_map/unordered_map/) (hash map) stores elements formed by the combination of a key value and a mapped value. The elements in the unordered_map are not sorted in any particular order with respect to either their key or mapped values, but organized into buckets depending on their hash values to allow for fast access to individual elements directly by their key values.

-----
- "int" and "const int&" can not be used as lvalue, but "int&" can.
- copy elision
