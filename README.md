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

Inheritance and Polymorphism (using a single interface to entities of different types):

9-a) For polymorphism, "virtual" **must** be used for the base class function declaration (for the first appearance). In the derived class the function is virtual by way of having the same type as the base class function. Optionally, "virtual" keyword in the derived class ensures that the function is still virtual in the further derived classes. 

9-b) "override" ensures that the function is overriding a virtual function from a base class. A compile-time error is generated if this is not true. Without "override", when the function is ill-formed, the closest virtual function in the inheritance hierarchy is called (no compilation error).

9-c) "final" ensures that a virtual memebr function can not be overriden by derived classes. Similarly, "final", in class definition, specifies that no classes can be dervied from this class.

9-d)  **Virtual destructors are useful when you might potentially delete an instance of a derived class through a pointer to the base class. In most implementations, the call to the destructor will be resolved like any non-virtual code, meaning that the destructor of the base class will be called but not the one of the derived class, resulting in a resources leak.**


9-e) With polymorphism, i.e., calling a derived class through a pointer/refrence to its base class, we can only use member functions that are provided by the base class.

9-f) A pure virtual function is declared by assigning "0" to it (i.e., it has no implementation). A class with one pure virtual function is abstract, i.e., no direct object from this class can be created (references or pointers for polymorphism are allowed). We can assign "default" to copy/move constructor or assignment operator to tell the compiler to create the default version of the respective constructor or assignment operator. It's better to let the compiler handle it than to implement it by ourselves. On the contrary, "delete" can be used when we don't want the compiler to generate that function automatically.

9-g) From software architecture point of view, composition with dependency injection seems better that direct inheritance (more flexibility and scalability).
![image](https://github.com/mhdslh/notes---C-/assets/61638154/077bdeba-9053-4cbf-ad7c-5de5dfdcdef6)

10- string_view (`usgin string_view = std::basic_string_view<char>`) refers to a constant contiguous sequence of char (e.g., a string_literal(C style string) or string class object). A typical implementation of string_view holds only two members: a pointer to **constant** char (thus can not modify the data) and a size. string_view does not own the data. As a result, if the original data gets modified, this will be reflected in the string_view object as well. 

Note that when using auto keyword the inferred type of a string literal **"*char-seq*"** is const char*; to make it a string we must use string object literals **"*char-seq*"s** as in the example below. To use string object literals we need `using namespace std::literals;`.
![image](https://github.com/mhdslh/notes---C-/assets/61638154/171bba94-5cf7-461f-85fb-e4f61ab99956)

11- unique_ptr, shared_ptr, weak_ptr
12- Copy constructor, move constructor, copy assignment, move assignment, "{}" operator overloading (member initializing list), pre and post increment operator overloading
13- move semantics, lvalue, rvalue, lvalue&, rvalue&, lvalue&&, rvalue&&  

14- Lambda expressions and function objects

  
Data Structures:
- [**std::vector**](https://en.cppreference.com/w/cpp/container/vector) The elements of a vector are stored contiguously, i.e., vector is not implemented as a linked list (refer to [std::list](https://en.cppreference.com/w/cpp/container/list)).  
- [**std::set**](https://en.cppreference.com/w/cpp/container/set) stores unique elements following a specific order. "set" containers are generally slower than "std::unordered_set" containers to access individual elements by their key. It is typically implemented as binary search trees.
- [**std::unordered_set**](https://en.cppreference.com/w/cpp/container/unordered_set) (hash set) stores unique elements in no particular order. Elements in unordered_set containers are organized into buckets depending on their hash values to allow for fast access to individual elements directly by their values.
- [**std::map**](https://en.cppreference.com/w/cpp/container/map) stores elements formed by a combination of a key value and a mapped value. The elements in a map are always sorted by its key following a specific order. Maps are typically implemented as binary search trees.
- [**std::unordered_map**](https://en.cppreference.com/w/cpp/container/unordered_map) (hash map) stores elements formed by the combination of a key value and a mapped value. The elements in the unordered_map are not sorted in any particular order with respect to either their key or mapped values, but organized into buckets depending on their hash values to allow for fast access to individual elements directly by their key values.

- Most of the binary search tree (BST) operations (e.g., search, insert, delete etc.) take O(h) time where h is the height of the BST. The cost of these operations may become O(n) (i.e., linear) for a skewed Binary tree. If we make sure that the height of the tree remains O(log(n)) after every insertion and deletion, then we can guarantee an upper bound of O(log(n)) for all these operations. With n denoting the number of nodes in the tree, the height of Red-Black tree is always less than 2log2(n+1), and the height of an AVL tree is always 1.44log2(n). Therefore, AVL tree is more balanced; however, AVL trees are more expensive at insertion and deletetion (to rotate the tree to meet the required properties). std::set and std::map in C++ are usually implemented as Red-Black trees. B-tree is a self-balancing tree data structure that allows nodes to store more than one key and have number of keys plus 1 children. This provides a shallower height, and less disk I/O as a result. B-Trees are particularly well suited for storage systems that have slow bulky data access such as hard drives, flash memory, and CD-ROMs.
- ordered hash map = hash set + queue (must be in sync)

- A tree can be represented as `(representation of the left subtree) root.val (representation of the right subtree)`.
![image](https://github.com/mhdslh/notes---C-/assets/61638154/64a2219a-720d-4f62-8839-7adf77aaff36)

- Singly-linked list ([**std::forward_list**](https://en.cppreference.com/w/cpp/container/forward_list)) and Doubly-linked list  ([**std::forward_list**](https://en.cppreference.com/w/cpp/container/list)):

- Heap with priority queue

-----
- erase in hashmap (iterators)
- covariance
- "int" and "const int&" can not be used as lvalue, but "int&" can.
- copy elision
