# notes---C++

Object-oriented programming:

1- For contructors that **can** be called with one argument, we must use explicit to avoid implicit convert types. If a class has a constructor which can be called with a single argument, then this constructor becomes a **conversion constructor** because such a constructor allows conversion of the single argument to the class being constructed. To avoid such implicit conversions as these may lead to unexpected results, we can make the constructor explicit.

2- For every class, compiler automatically creates a default **assignment operator** (operator=) and **copy constructor** (a constructor which can be called with an argument of the same class type).

3- **Conversion operator** can be used to convert a class type to another type ([reference](https://en.cppreference.com/w/cpp/language/cast_operator)).

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

11- **lvalue** refers to a memory location that identifies an object (identifier). Expressions referring to modifiable locations are called "modifiable l-values". On the other hand, **rvalue** refers to a temporary object (**prvalue** has no identifiable location in memory, but **xvalue** represents an object whose resources can be 'moved' or reused). **glvalue** (Generalized lvalue) includes both lvalues (objects with a distinct identity and location) and xvalues. An xvalue is considered a glvalue because it refers to an object, albeit an object that is in a state where its resources are about to be reused or moved. This means that an xvalue has an identity, like an lvalue, but is also in a state where it can be treated like a temporary or movable object, similar to an rvalue.

<img src="https://github.com/mhdslh/notes---C-/assets/61638154/8b669a66-5e32-46a6-a2fc-0c855c7032d3" width=40% height=40%>

A [reference](https://en.cppreference.com/w/cpp/language/reference#Lvalue_references) is required to be initialized at the time of declaration. Once initialized, a reference cannot be reseated (changed) to refer to another object. Doing so modifies the original object based on the new object.
- **lvalue references** (`S& D`) can be used to 1) alias an existing object, 2) implement pass-by-reference semantics, 3) use function call expression as an lvalue when the function's return type is lvalue reference.
- **rvalue references** (`S&& D`) can be used to extend the lifetimes of temporary objects (same can be achieved by const l-value references; however, they are not modifiable).

lvalue references can only take lvalues unless they are constant. rvalue references can only take rvalues. When a function has both rvalue reference and lvalue reference overloads, the rvalue reference overload binds to rvalues, while the lvalue reference overload binds to lvalues. This allows move constructors, move assignment operators, and other move-aware functions (e.g. std::vector::push_back()) to be automatically selected when suitable. The std::move function is a standard library utility that converts its argument into an rvalue reference, specifically an xvalue. This conversion signals that the resources held by the argument can be moved. When you pass an object to std::move, it is cast to an rvalue reference. This casting doesn't actually move anything by itself; it just enables the move operation. This rvalue reference is typically an xvalue, indicating that the resources of the object can be moved. In the presence of an rvalue reference (particularly an xvalue), the compiler can choose to invoke move constructors or move assignment operators of objects.

Note: When a variable is declared as T&& t, it is an rvalue reference. However, this reference itself is an lvalue because it has a name and can be addressed.

Class T {\
public:\
    T()                                               // default constructor: constructor that can be called with no arguments\
    T(*single-parameter*)                             // conversion constructor: contructor that can be called with one argument\
    T(T&)             , T(const T&)             , ... // [copy constructor](https://en.cppreference.com/w/cpp/language/copy_constructor)\
    T(T&&)            , T(const T&&)            , ... // [move constructor](https://en.cppreference.com/w/cpp/language/move_constructor)\
    T& operator=(T&)  , T& operator=(const T&)  , ... // [copy assignment](https://en.cppreference.com/w/cpp/language/copy_assignment)\
    T& operator=(T&&) , T& operator=(const T&&) , ... // [move assignment](https://en.cppreference.com/w/cpp/language/move_assignment)\
    ~T()                                              // destructor\
};

12- unique_ptr, shared_ptr, weak_ptr

13- **RAII (Resource Acquisition Is Initialization)** and **Rule of Zero**: RAII involves wrapping resources into classes, where the resource is acquired in the constructor and released in the destructor. This ensures that resources are properly released when the object goes out of scope, reducing memory leaks and other resource management errors. 

When we invoke one of the five special member functions (constructor, destructor, copy constructor, copy assignment operator, and move assignment operator) on a class, the corresponding special member function for each of its composed (non-static member) objects is also called. When a class is only composed of RAII objects and no other resources and its special member function is called, the corresponding member function of those RAII member objects are automatically invoked. This ensures proper resource management. As a result, there is no need for custom special member functions and the default implementations provided by the compiler are sufficient. The "Rule of Zero" suggests that classes should avoid custom destructors, copy/move constructors, and copy/move assignment operators. Instead, they should use existing classes that follow the RAII principle to manage resources.

14- Containers:
- **sequence containers**: array, vector, deque, list (doubly-linked list), forward-list (singly-linked list)
- **ordered associative containers**: set, multiset, map, multimap (implemented by binary search trees)
- **unordered associative containers**: unordered-set , unordered-multiset, unordered-map, unordered-multimap (implemented by hash functions)
- **container adaptors**: stack, queue, priority_queue

To use a container, its elements’ type must meet a minimum certain requirements, e.g., CopyConstructible, MoveConstructible, CopyAssignable, MoveAssignable, EqualityComparable, etc. (for more information refer to this [link](https://en.cppreference.com/w/cpp/named_req)). Moreover, each container supports a [catergory of iterators](https://en.cppreference.com/w/cpp/iterator) from which we can choose the right algorithms to work with the container.
![image](https://github.com/mhdslh/notes---C-/assets/61638154/7fde2041-36fe-4d40-933b-75075cac2f79)

15- A C++ 20 range is essentially an abstraction representing a sequence of elements, like an array, a linked list, a vector, or any other container. The key idea behind ranges is to provide a more unified and powerful approach to iterating over and manipulating these sequences. For instance, input range is a range that supports input_iterator, a concept introduced in iterator header. std::range version of standard algorithms takes ranges as argument. Range adaptors can be thought of as operations that can be applied to a range to produce a new view. These adaptors can be chained together to perform complex views. While any object that can be iterated over (i.e., has a beginning and an end) is considered a range, view is a non-owning wrapper around a range. Views do not own the elements they refer to; instead, they provide a different way to "view" or process the data of another range. Views are lazily evaluated (results are computed only when needed).

16- A function object is any object for which the function call operator (operator()) is defined. When defining lambda functions (having format '[ capture_clause ] (parameters) -> return_type { body }'), compiler automatically generates an anonymous class behind the scenes with operator() overloaded and captured variables, in the lambda expression, stored as member variables in the generated class. The lambda expression is effectively an instance of this class, i.e., lambdas are implemented as function objects. Projection concept in std::ranges algorithms is a function or a callable object that transforms elements of a range into another form before they are processed by an algorithm. This allows algorithms to operate not directly on the elements of the range, but on a certain aspect or property of these elements ([similar to projection in mathematics](https://mathworld.wolfram.com/Projection.html)). Projection can be combined with built-in function objections to support more use cases. 

![image](https://github.com/mhdslh/notes---C-/assets/61638154/6d6fea0a-c345-4621-a146-8f3272c3096c)

17- Structure binding, introduced in C++17, simplifies unpacking elements from tuples, pairs, arrays, and structures. As an example: <br>

std::pair<int, std::string> p{1, "Hello"}; <br>
auto [number, text] = p;

18-a) - Templates:
![image](https://github.com/mhdslh/notes---C-/assets/61638154/173acabc-3e41-4ac8-a6e5-e56d708b9a99)
![image](https://github.com/mhdslh/notes---C-/assets/61638154/7c536bcd-36e7-4a5d-9f3b-2ba5b73981bb)

19- multi inheritance

20- 
  niceness
  boost::fibers::SynchExecutor<void> executor;
  boost::fibers::algo::SetSchedPolicy();


---
Data Structures:
- [**std::vector**](https://en.cppreference.com/w/cpp/container/vector) The elements of a vector are stored contiguously, i.e., vector is not implemented as a linked list (refer to [std::list](https://en.cppreference.com/w/cpp/container/list)).  
- [**std::set**](https://en.cppreference.com/w/cpp/container/set) stores unique elements following a specific order. "set" containers are generally slower than "std::unordered_set" containers to access individual elements by their key. It is typically implemented as binary search trees.
- [**std::unordered_set**](https://en.cppreference.com/w/cpp/container/unordered_set) (hash set) stores unique elements in no particular order. Elements in unordered_set containers are organized into buckets depending on their hash values to allow for fast access to individual elements directly by their values.
- [**std::map**](https://en.cppreference.com/w/cpp/container/map) stores elements formed by a combination of a key value and a mapped value. The elements in a map are always sorted by its key following a specific order. Maps are typically implemented as binary search trees.
- [**std::unordered_map**](https://en.cppreference.com/w/cpp/container/unordered_map) (hash map) stores elements formed by the combination of a key value and a mapped value. The elements in the unordered_map are not sorted in any particular order with respect to either their key or mapped values, but organized into buckets depending on their hash values to allow for fast access to individual elements directly by their key values.

- Most of the binary search tree (BST) operations (e.g., search, insert, delete etc.) take O(h) time where h is the height of the BST. The cost of these operations may become O(n) (i.e., linear) for a skewed Binary tree. If we make sure that the height of the tree remains O(log(n)) after every insertion and deletion, then we can guarantee an upper bound of O(log(n)) for all these operations. With n denoting the number of nodes in the tree, the height of Red-Black tree is always less than 2log2(n+1), and the height of an AVL tree is always 1.44log2(n). Therefore, AVL tree is more balanced; however, AVL trees are more expensive at insertion and deletetion (to rotate the tree to meet the required properties). std::set and std::map in C++ are usually implemented as Red-Black trees. B-tree is a self-balancing tree data structure that allows nodes to store more than one key and have number of keys plus 1 children. This provides a shallower height, and less disk I/O as a result. B-Trees are particularly well suited for storage systems that have slow bulky data access such as hard drives, flash memory, and CD-ROMs.

- A binary tree can be represented as:
-     Pre-order Traversal: `root.val(representation of the left subtree)(representation of the right subtree)`.
-     Post-order Traversal: `(representation of the left subtree)(representation of the right subtree)root.val`.
-     In-order Traversal: `(representation of the left subtree) root.val (representation of the right subtree)`.
![image](https://github.com/mhdslh/notes---C-/assets/61638154/64a2219a-720d-4f62-8839-7adf77aaff36)

- A binary search tree (BST) is a binary tree with properties: 1) The value in each node must be greater than (or equal to) any values stored in its left subtree. 2) The value in each node must be less than (or equal to) any values stored in its right subtree. Inorder traversal in BST will be in ascending order. BST is a good candidate to store data in order.

- Singly-linked list ([**std::forward_list**](https://en.cppreference.com/w/cpp/container/forward_list)) and Doubly-linked list  ([**std::forward_list**](https://en.cppreference.com/w/cpp/container/list)):

- Queue (FIFO) and Stack (LIFO):\
[**std::queue**](https://en.cppreference.com/w/cpp/container/queue)\
[**std::deque**](https://en.cppreference.com/w/cpp/container/deque): indexed sequence container that allows fast insertion and deletion at both its beginning and its end. As opposed to std::vector, the elements of a deque are not stored contiguously: typical implementations use a sequence of individually allocated fixed-size arrays, with additional bookkeeping, which means indexed access to deque must perform two pointer dereferences.\
[**std::priority_queue**](https://en.cppreference.com/w/cpp/container/priority_queue) provides constant time lookup of the largest (by default) element (Note that because the priority queue outputs largest elements first, the elements that "come before", according to the ordering imposed by Compare,  are actually output last)\
[**std::stack**](https://en.cppreference.com/w/cpp/container/stack)
  
- Breadth-first Search (BFS) and Depth-first Search (DFS) algorithms can be used to traverse or search in data structures like trees or graphs. Queue is used in implementing BFS algorithms, and Stack is used in implementing DFS algorithms.

- Heap with priority queue and priority queue with heap

-----
- erase in hashmap (iterators)
- covariance
- copy elision
