## Iterators
- ES6 has several important features that help significantly improve properly organize JavaScript code, including:
  - Iterators
  - Generators
  - Modules
  - Classes
- ES6 has introduced an implicit standardized interface for iterators
- Many built-in data structures will expose an iterator

> Organization: Iterators are a way of organizing ordered, sequential, pull-based consumption of data

### Interfaces
1. `Iterator` Interface
```
Iterator [required]
    next() {method}: retrieves next IteratorResult
```

2. `IteratorResult` Interface
```
{ value: .. , done: true / false }
```
