---
title: Timing and Benchmarking
description: Timing and benchmarking of the Julia language.
author: karei
date: 2025-01-17 00:00:00 +0000
categories: [Blogging]
tags: [tutorial, programming, julia]
pin: false
media_subpath: '/posts/20250117'
---

## Timing

**time()**

`time()` function gets the system time in seconds since the epoch, with fairly high (typically, microsecond) resolution.

```julia
start_time = time()
# ...
elapsed_time = time() - start_time
println("Elapsed time: $elapsed_time seconds")
```

**@elapsed**

`@elapsed` is a macro to evaluate an expression, discarding the resulting value, instead returning the number of seconds it took to execute as a floating-point number.

```julia
elapsed_time = @elapsed begin
    # ...
end
println("Elapsed time: $elapsed_time seconds")
```

**TimerOutputs.jl**

[TimerOutputs](https://github.com/KristofferC/TimerOutputs.jl) is a small Julia package that is used to generate formatted output from timings made in different sections of a program.

```julia
using TimerOutputs
const to = TimerOutput() # create a timer
# Define a function that needs to be timed
function compute_sum(n)
    sum = 0
    @timeit to "Loop over elements" for i in 1:n
        sum += i
    end
    return sum
end
@timeit to "Total computation" begin
    result1 = compute_sum(100_000)
    result2 = compute_sum(200_000)
    println("Result 1: ", result1)
    println("Result 2: ", result2)
    show(to)
end
```

## Benchmarking

```julia
using BenchmarkTools
a = rand(100,100)
b = rand(100,100)
@btime begin
    c = $a + $b
end
```

A description of “$” can be found at [stackoverflow](https://stackoverflow.com/questions/68261871/what-is-the-dollar-sign-prefix-in-function-arguments-used-for-in-julia).
