---
author: "UmmIt"
title: "Python basic"
description: ""
tags: ["Python"]
date: 2024-12-16T02:11:00+0800
---

## Python Cheat Sheet

### 1. Basic Syntax
- **Print Statement**: 
  ```python
  print("Hello, World!")
  ```

- **Comments**: 
  ```python
  # Single-line comment
  """
  Multi-line comment
  """
  ```

### 2. Data Types
- **Numeric Types**: 
  - `int`: Integer values
  - `float`: Floating-point values
  - `complex`: Complex numbers
  ```python
  a = 5          # int
  b = 3.14       # float
  c = 1 + 2j     # complex
  ```

- **Strings**: 
  ```python
  str_var = "Hello, World!"
  ```

- **Booleans**: 
  ```python
  is_true = True
  ```

- **Data Structures**:
  - **Lists**: Ordered, mutable
    ```python
    my_list = [1, 2, 3, "four"]
    ```
  - **Tuples**: Ordered, immutable
    ```python
    my_tuple = (1, 2, 3)
    ```
  - **Sets**: Unordered, unique elements
    ```python
    my_set = {1, 2, 3}
    ```
  - **Dictionaries**: Key-value pairs
    ```python
    my_dict = {"name": "Alice", "age": 25}
    ```

### 3. Control Structures
- **Conditional Statements**: 
  ```python
  if condition:
      # do something
  elif another_condition:
      # do something else
  else:
      # do something different
  ```

- **Loops**:
  - **For Loop**: 
    ```python
    for item in iterable:
        # do something
    ```
  - **While Loop**: 
    ```python
    while condition:
        # do something
    ```

### 4. Functions
- **Defining Functions**: 
  ```python
  def function_name(parameters):
      # function body
      return value
  ```

- **Lambda Functions**: 
  ```python
  square = lambda x: x ** 2
  ```

- **Docstrings**: 
  ```python
  def my_function():
      """This function does something."""
      pass
  ```

### 5. Exception Handling
```python
try:
    # code that may raise an exception
except ExceptionType as e:
    # handle exception
finally:
    # cleanup code
```

### 6. File Handling
- **Reading a File**: 
  ```python
  with open('file.txt', 'r') as file:
      content = file.read()
  ```

- **Writing to a File**: 
  ```python
  with open('file.txt', 'w') as file:
      file.write("Hello, World!")
  ```

### 7. List Comprehensions
```python
squared = [x**2 for x in range(10)]
filtered = [x for x in my_list if x > 2]
```

### 8. Common Built-in Functions
- **Length**: 
  ```python
  len(my_list)
  ```

- **Range**: 
  ```python
  list(range(0, 10, 2))  # [0, 2, 4, 6, 8]
  ```

- **Type Conversion**: 
  ```python
  int("123")
  float("123.45")
  str(123)
  ```

### 9. Common Libraries
- **NumPy**: For numerical operations
  ```python
  import numpy as np
  array = np.array([1, 2, 3])
  ```

- **Pandas**: For data manipulation
  ```python
  import pandas as pd
  df = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
  ```

- **Matplotlib**: For plotting
  ```python
  import matplotlib.pyplot as plt
  plt.plot([1, 2, 3], [4, 5, 6])
  plt.show()
  ```

### 10. Object-Oriented Programming
- **Class Definition**: 
  ```python
  class MyClass:
      def __init__(self, value):
          self.value = value
      
      def display(self):
          print(self.value)
  ```

- **Inheritance**: 
  ```python
  class ChildClass(MyClass):
      def __init__(
