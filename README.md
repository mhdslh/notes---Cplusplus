# notes---C++

1- For contructors that **can** be called with one argument, we must use explicit to avoid implicit convert types. If a class has a constructor which can be called with a single argument, then this constructor becomes a conversion constructor because such a constructor allows conversion of the single argument to the class being constructed. To avoid such implicit conversions as these may lead to unexpected results, we can make the constructor explicit.
