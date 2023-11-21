# notes---C++

1- For contructors that **can** be called with one argument, we must use explicit to avoid implicit convert types. If a class has a constructor which can be called with a single argument, then this constructor becomes a **conversion constructor** because such a constructor allows conversion of the single argument to the class being constructed. To avoid such implicit conversions as these may lead to unexpected results, we can make the constructor explicit.

2- For every class, compiler automatically creates a default **assignment operator** (operator=) and **copy constructor** (a constructor which can be called with an argument of the same class type).

3- **Conversion Operator** can be used to convert a class type to another type ([reference](https://en.cppreference.com/w/cpp/language/cast_operator)).

4- [Casting in C++](https://stackoverflow.com/questions/28002/regular-cast-vs-static-cast-vs-dynamic-cast): static cast performs conversion between compatible types for example to reverse an implicit conversion. It is similar to the C-style cast, but is more restrictive. Static cast performs no runtime checks. Dynamic cast is only used to convert object pointers or object references into other pointer or reference types in the inheritance hierarchy. Dynamic cast has runtime type checking.
