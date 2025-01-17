---
title: Inputs and outputs
description: >-
  Basic input and output of the Julia language.
author: karei
date: 2025-01-17 00:00:00 +0000
categories: [Blogging]
tags: [tutorial, programming, julia]
pin: false
media_subpath: '/posts/20250117'
---

## Clear screen

```julia
print("\033c")
```

## Suspension

```julia
println("Press any key to continue...")
readline()
println("Continuing execution...")
```

## Reading input from a terminal

```julia
input = stdin |> readline |> split |> x -> parse.(Int, x)
```

## Output data to terminal

There are many ways to print a matrix, and I'm going to cover all the ones I know of.

```julia
matrix = [0.0 2.3456 20.0;
    4.5678 5.0 123456.7890;
    0.001234 123.456 0.0000078901]
```

The most straightforward way is to enter the variable name `matrix` into the REPL, which is equivalent to `display(matrix)` and `@info "Message" matrix`, they all call `Base.show(stdout, MIME("text/plain"), matrix)`.

We will get the following output:

```terminal
3×3 Matrix{Float64}:
 0.0         2.3456  20.0
 4.5678      5.0      1.23457e5
 0.001234  123.456    7.8901e-6
```

> `MIME(“text/plain”)` indicates a human-readable log format. 
{: .prompt-info }

Another set of output functions are `print(matrix)`, `println(matrix)`, and `show([io], matrix)`, they all call `Base.show(stdout, matrix)`. **Their output is more concise and easy to use as input/output for program data streams:**

```2
[0.0 2.3456 20.0; 4.5678 5.0 123456.789; 0.001234 123.456 7.8901e-6]
```

To print all decimals, use `Base.print_matrix(stdout, matrix)`

```terminal
 0.0         2.3456      20.0
 4.5678      5.0     123456.789
 0.001234  123.456        7.8901e-6
```

## Formatted output

If the numbers in the array are not on the same order of magnitude, or if some of the numbers are integers but the whole array is `Float64`, these integers are forced to be appended with a ".0", which makes it hard to read, such as the above variable `matrix`. The following line of code allows you to pretty print the matrix in `"%7.3g  "` format.

```julia
using Printf
# for a matrix, one can use
foreach(row -> (foreach(x -> @printf("%7.3g ", x), row); println()), eachrow(matrix))
# for a vector, use
println("matrix=[" * join(map(x -> @sprintf("%.4f", x), matrix), "  ") * "]")
```

In this way, the integers in the matrix are not accompanied by “.0” and the length of each number is controlled.

```2terminal
      0    2.35      20
   4.57       5 1.23e+05
0.00123     123 7.89e-06
```

### Using PrettyTables.jl

PrettyTables.jl provides a format for printing matrices specifically, and the same can be done with `"%7.3g  "` for pretty printing.

```julia
using PrettyTables
pretty_table(matrix; tf=tf_matrix, show_header=false, formatters=ft_printf("%.3g"), display_size=(-1, -1))
```

```2
┌                         ┐
│       0  2.35        20 │
│    4.57     5  1.23e+05 │
│ 0.00123   123  7.89e-06 │
└                         ┘
```

> The `display_size=(-1, -1)` statement allows you to print the entire matrix, whether the terminal is large enough or not. However, when the terminal is not large enough to print the matrix using the built-in function above, part of the matrix will be omitted.
{: .prompt-info }

### Using Format.jl

To put a comma between every third number, use the following method:

```julia
using Format
s = cfmt("%'d", 123456789) # fast
s = format(123456789, commas=true) # medium
```

Output is a string:`"123,456,789"`

## Output to VS code's table window

`vscodedisplay()` opens a new window in VS code that displays the variables in a tabular form, for example:

```julia
vscodedisplay(matrix)
```

![Desktop View](/01.png){: width="320" .normal}
