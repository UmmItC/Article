---
author: "UmmIt"
title: "Python data structures"
description: ""
tags: ["Python", "Conditionals"]
date: 2024-12-16T02:07:00+0800
---

# Python Data Structures Cheatsheet

## Mutable Data Structures
These data structures can be changed after they are created.

1. **List**
   ```python
   my_list = [1, 2, 3, 4, 5]
   my_list.append(6)  # You can modify the list
   print(my_list)  # Output: [1, 2, 3, 4, 5, 6]
   ```

2. **Dictionary**
   ```python
   my_dict = {'name': 'Alice', 'age': 25}
   my_dict['age'] = 26  # You can modify the dictionary
   print(my_dict)  # Output: {'name': 'Alice', 'age': 26}
   ```

3. **Set**
   ```python
   my_set = {1, 2, 3}
   my_set.add(4)  # You can modify the set
   print(my_set)  # Output: {1, 2, 3, 4}
   ```

4. **Deque (from collections)**
   ```python
   from collections import deque
   my_queue = deque([1, 2, 3])
   my_queue.append(4)  # You can modify the deque
   print(my_queue)  # Output: deque([1, 2, 3, 4])
   ```

5. **Stack (using list)**
   ```python
   my_stack = []
   my_stack.append(1)  # You can modify the stack
   my_stack.append(2)
   popped_item = my_stack.pop()  # Remove the top item
   print(my_stack)  # Output: [1]
   print(popped_item)  # Output: 2
   ```

## Immutable Data Structures
These data structures cannot be changed after they are created.

1. **Tuple**
   ```python
   my_tuple = (1, 2, 3)
   try:
       my_tuple[0] = 4  # Attempt to modify the tuple
   except TypeError as e:
       print(e)  # Output: 'tuple' object does not support item assignment
   ```

2. **String**
   ```python
   my_string = "Hello"
   try:
       my_string[0] = "h"  # Attempt to modify the string
   except TypeError as e:
       print(e)  # Output: 'str' object does not support item assignment
   ```

3. **Frozenset**
   ```python
   my_frozenset = frozenset([1, 2, 3])
   try:
       my_frozenset.add(4)  # Attempt to modify the frozenset
   except AttributeError as e:
       print(e)  # Output: 'frozenset' object has no attribute 'add'
   ```

## Summary
- **Mutable**: List, Dictionary, Set, Deque, Stack (using list)
- **Immutable**: Tuple, String, Frozenset
