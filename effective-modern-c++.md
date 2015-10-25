# Effective Modern C++ CheatSheet

## Chapter 1. Deducing Types
- Item 1: Understand template type deduction.
- Item 2: Understand `auto` type deduction.
- Item 3: Understand `decltype`.
- Item 4: Know how to view deduced types.

## Chapter 2. `auto`
- Item 5: Prefer `auto` to explicit type declarations.
- Item 6: Use the explicitly typed initializer idiom when `auto` deduces undesired types.

## Chapter 3. Moving to Modern C++
- Item 7: Distinguish between () and {} when creating objects.
- Item 8: Prefer `nullptr` to 0 and NULL.
- Item 9: Prefer alias declarations to `typedef`s.
- Item 10: Prefer scoped `enum`s to unscoped `enum`s.
- Item 11: Prefer deleted functions to private undefined ones.
- Item 12: Declare overriding functions `override`.
- Item 13: Prefer `const_iterators` to `iterators`.
- Item 14: Declare functions `noexcept` if they won't emit exceptions.
- Item 15: Use `constexpr` whenever possible.
- Item 16: Make `const` member functions thread safe.
- Item 17: Understand special member function generation.
  - C++98: ctor, dtor, copy ctor, copy assignment opeartor
  - C++11:
    - `Foo(Foo&& rhs);`
    - `Foo& operator=(Foo&& rhs);`
  - The special member functions are those compilers may generate on their own: default constructor, destructor, copy operations, and move operations.
  - Move operations are generated only for classes lacking explicitly declared move operations, copy operations, and a destructor.
  - The copy constructor is generated only for classes lacking an explicitly declared copy constructor, and it's deleted if a move operation is declared. The copy assignment operator is generated only for classes lacking an explicitly declared copy assignment operator, and it's deleted if a move operation is declared. Generation of the copy operations in classes with an explicitly declared destructor is deprecated.
  - Member function templates never suppress generation of special member functions.

## Chapter 4. Smart Pointers
- Item 18: Use `std::unique_ptr` for exclusive-ownership resource management.
- Item 19: Use `std::shared_ptr` for shared-ownership resource management.
- Item 20:  Use `std::weak_ptr` for `std::shared_ptr`-like pointers that can dangle.
  - Compared to direct use of new, make functions eliminate source code duplication, improve exception safety, and, for `std::make_shared` and `std::allocate_shared`, generate code that's smaller and faster.
  - Situations where use of make functions is inappropriate include the need to specify custom deleters and a desire to pass braced initializers.
  - For `std::shared_ptr`s, additional situations where make functions may be ill-advised include (1) classes with custom memory management and (2) systems with memory concerns, very large objects, and `std::weak_ptr`s that outlive the corresponding `std::shared_ptr`s.

## Chapter 5. Rvalue References, Move Semantics, and Perfect Forwarding
- Item 23: Understand `std::move` and `std::forward`.
  - `std::move` performs an unconditional cast to an rvalue. In and of itself, it doesn't move anything.
  - `std::forward` casts its argument to an rvalue only if that argument is bound to an rvalue.
  - Neither `std::move` nor `std::forward` do anything at runtime.
- Item 24: Distinguish universal references from rvalue references.
  - If a function template parameter has type `T&&` for a deduced type `T`, or if an object is declared using `auto&&`, the parameter or object is a universal reference.
  - If the form of the type declaration isn't precisely `type&&`, or if type deduction does not occur, `type&&` denotes an rvalue reference.
  - Universal references correspond to rvalue references if they're initialized with rvalues. They correspond to lvalue references if they're initialized with lvalues.
- Item 25: Use `std::move` on rvalue references, `std::forward` on universal references.
  - Apply `std::move` to rvalue references and `std::forward` to universal references the last time each is used.
  - Do the same thing for rvalue references and universal references being returned from functions that return by value.
  - Never apply `std::move` or `std::forward` to local objects if they would otherwise be eligible for the return value optimization.
- Item 26: Avoid overloading on universal references.
- Item 27: Familiarize yourself with alternatives to overloading on universal references.
- Item 28: Understand reference collapsing.
- Item 29: Assume that move operations are not present, not cheap, and not used.
  - In code with known types or support for move semantics, there is no need for assumptions.
- Item 30: Familiarize yourself with perfect forwarding failure cases.
  - The kinds of arguments that lead to perfect forwarding failure are braced initializers, null pointers expressed as 0 or NULL, declaration-only integral const static data members, template and overloaded function names, and bitfields.

## Chapter 6. Lambda Expressions
- Item 31: Avoid default capture modes.
- Item 32: Use init capture to move objects into closures.
- Item 33: Use `decltype` on `auto&&` parameters to `std::forward` them.
- Item 34: Prefer lambdas to `std::bind`

## Chapter 7. The Concurrency API
- Item 35:  Prefer task-based programming to thread-based.
- Item 36: Specify `std::launch::async` if asynchronicity is essential.
- Item 37:  Make `std::threads` unjoinable on all paths.
- Item 39: Consider void futures for one-shot event communication.
- Item 40: Use `std::atomic` for concurrency, `volatile` for special memory.

## Chapter 8. Tweaks
- Item 41: Consider pass by value for copyable parameters that are cheap to move and always copied.
- Item 42: Consider emplacement instead of insertion.

