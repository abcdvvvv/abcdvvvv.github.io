---
title: Control Flow
description: Different ways to write for loop in Julia lang.
author: karei
date: 2025-01-17 00:00:00 +0000
categories: [Blogging]
tags: [tutorial, programming, julia]
pin: false
media_subpath: '/posts/20250117'
---

## The for loop

To iterate over the elements within an array, there are several different ways to write the code. Now suppose we have such an array: `a = copy(reshape(1:16, 4, 4))`

```2
4×4 Matrix{Int64}:
 1  5   9  13
 2  6  10  14
 3  7  11  15
 4  8  12  16
```

### 1. eachindex(a)

In Julia, matrices are stored column-first. Hence, `eachindex()` iterates over matrix a in column order.

- `eachindex()` returns the index of each element of a matrix, of type `Int`

```julia
for i in eachindex(a)
    println("Index: $i, Element: $(a[i])")
end
```

```2
Index: 1, Element: 1
Index: 2, Element: 2
Index: 3, Element: 3
Index: 4, Element: 4
Index: 5, Element: 5
Index: 6, Element: 6
...
```

### 2. a

- `in a` returns each element of a matrix, of type `eltype(a)`

```julia
for element in a
    println("Element: $element")
end
```

### 3. pairs(a)

The `pairs()` function, when used as an iterator, can iterate not only over arrays, but also over Pairs and dictionaries.

- `pairs(a::Vector)` returns a pair of index and value, of type `Tuple{Int,eltype(a)}`
- `pairs(a::Matrix)` returns a pair of index and value, of type `Tuple{CartesianIndex{2},eltype(a)}`
<!-- - `pairs(::Tuple{Pair})`, `pairs(::Vector{Pair})` returns a pair of key and value, of type `Tuple{eltype(a[i].first),eltype(a[i].second)}` -->
- `pairs(::Dict)` returns a pair of key and value, of type `Tuple{eltype(a.keys),eltype(a.vals)}`

A `[println("Index: $i, Element: $element") for (i, element) in pairs(a)]` gives:

```2
Index: CartesianIndex(1, 1), Element: 1
Index: CartesianIndex(2, 1), Element: 2
Index: CartesianIndex(3, 1), Element: 3
Index: CartesianIndex(4, 1), Element: 4
Index: CartesianIndex(1, 2), Element: 5
Index: CartesianIndex(2, 2), Element: 6
...
```

### 4. enumerate(a)

The first parameter *i* produced by `enumerate` is the index of natural number starting from 1.

A `[println("Index: $i, Element: $element") for (i, element) in enumerate(a)]` gives:

```2
Index: 1, Element: 1
Index: 2, Element: 2
Index: 3, Element: 3
Index: 4, Element: 4
Index: 5, Element: 5
Index: 6, Element: 6
...
```

### 5. axes(a,n)

This function iterates over the *n*th axis of array `a`.

```julia
for element in axes(a, 1)
    println("Element: $element")
end
```

```2
Element: 1
Element: 2
Element: 3
Element: 4
```

### Creating Iterators

An iterator can be created using parentheses wrapped around an array and a for loop -> `(a[i] for i in a)`

For example, we can create a row index iterator for matrix x:

```julia
x = rand(1000, 4);
eachrow_x = (x[i:i, :] for i in axes(x, 1));
eachrow_x = (view(x, i:i, :) for i in axes(x, 1));
eachrow_x = eachrow(x);
for (i, xi) in enumerate(eachrow_x)
    println("Index: $i, Element: $element")
    xi[1] = 0.0
end
```

- Line 2 let the for loop performs a slicing operation on the matrix x at each iteration, producing a new vector for which a memory allocation occurs.

- Lines 3 and 4 are equivalent iterators that let the for loop produce a row view of the matrix x per iteration. No memory allocation.
