---
title: "Using SoA over AoS in Go"
date: 2024-03-28T15:30:00-03:00
draft: true
---

When you want to work with data in a program, we often need to look around how
we are storing and accessing this date. For reasons of code complexity and
performance, we may prefer choosing one over the another. Go has a very opiniated
"idiomatic" approach on writing code, and also does lack some codegen features that
could improve performance, like using SIMD instructions for vectorized data. This
by itself could make someone argue that using Structure of Arrays (SoA) is basically
useless, but I want to show in this article that we can still benefit for using it
over Array of Structures (AoS)

# Testing Environment

I'll do the tests on my local PC, which is equipped with this hardware and software:

* Processor: Intel i5-9400F
* RAM: 2x16GB 2666MHz Dual Channel
* OS: Arch Linux with zen kernel 6.8
* Go: Version 1.22.1

Technically, my processor supports SSE 4.2 and AVX2, but the go compiler only uses
these instructions for memory allocation, not for codegen. For the benchmarks,
I'll use the excellent benchmark tools that the go toolchain ships.

# Code

I'll going to give three examples. One to calculate the area of one of the sides
of a cuboid, other that does a readjust of all employers' salary by 20% and the
same as the prior but calculating based on how many years since their salary
haven't got readjusted, so we can see the implications of branching in performance.

## Calculating the area of a 3D cuboid

Consider we have the following code for calculating the area of a side of a cube:

```go
type Cuboids struct {
	id []int64
	x  []int
	y  []int
	z  []int
}

type Cuboid struct {
	id int64
	x int
	y int
	z int
}
```

We're going to add an `id` and `z` field to show the implications of storing data
that we don't need when doing batches of calculations. This should benefit the
performance of SoA because we are storing in the CPU cache only the data we need
for the calculation, not the whole cube representation. The implications of this
will become more apparent on the next examples.

This is the test code we're going to use to benchmark:

```go
func BenchmarkAoS(b *testing.B) {
    aos := make([]Cuboid, b.N)
    for i := 0; i < b.N; i++ {
        aos[i] = Cuboid{int64(i), i, i, i}
    }

    result := make([]int, b.N)

    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        result[i] = aos[i].x * aos[i].y
    }
}

func BenchmarkSoA(b *testing.B) {
    soa := Cuboids {
        id: make([]int64, b.N),
        x: make([]int, b.N),
        y: make([]int, b.N),
        z: make([]int, b.N),
    }

    for i := 0; i < b.N; i++ {
        soa.id[i] = int64(i)
        soa.x[i] = i
        soa.y[i] = i
        soa.z[i] = i
    }

    result := make([]int, b.N)

    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        result[i] = soa.x[i] * soa.y[i]
    }
}
```

We just want to benchmark the time it takes to do the calculations, not memory
allocation, that's why I'm adding `b.ResetTimer()`. Also, the first test running
always takes a long time for some reason, so I'm gonna skip to make results more
consistent.

Here are the numbers:

```sh
$ go test -bench=. -count=5 -benchtime=1000x
goos: linux
goarch: amd64
pkg: github.com/robertoesteves13/aos-vs-soa
cpu: Intel(R) Core(TM) i5-9400F CPU @ 2.90GHz
BenchmarkAoS-6   	   1000	        0.9940 ns/op
BenchmarkAoS-6   	   1000	        0.9640 ns/op
BenchmarkAoS-6   	   1000	        0.9960 ns/op
BenchmarkAoS-6   	   1000	        1.035 ns/op
BenchmarkAoS-6   	   1000	        0.9570 ns/op
BenchmarkSoA-6   	   1000	        1.222 ns/op
BenchmarkSoA-6   	   1000	        1.223 ns/op
BenchmarkSoA-6   	   1000	        1.259 ns/op
BenchmarkSoA-6   	   1000	        1.196 ns/op
BenchmarkSoA-6   	   1000	        1.266 ns/op
PASS
ok  	github.com/robertoesteves13/aos-vs-soa	0.005s
```

```sh
$ go test -bench=. -count=5 -benchtime=1000000x
goos: linux
goarch: amd64
pkg: github.com/robertoesteves13/aos-vs-soa
cpu: Intel(R) Core(TM) i5-9400F CPU @ 2.90GHz
BenchmarkAoS-6   	1000000	        2.496 ns/op
BenchmarkAoS-6   	1000000	        1.911 ns/op
BenchmarkAoS-6   	1000000	        1.926 ns/op
BenchmarkAoS-6   	1000000	        1.915 ns/op
BenchmarkAoS-6   	1000000	        2.084 ns/op
BenchmarkSoA-6   	1000000	        1.437 ns/op
BenchmarkSoA-6   	1000000	        1.403 ns/op
BenchmarkSoA-6   	1000000	        1.367 ns/op
BenchmarkSoA-6   	1000000	        1.421 ns/op
BenchmarkSoA-6   	1000000	        1.386 ns/op
PASS
ok  	github.com/robertoesteves13/aos-vs-soa	0.084s
```

```sh
$ go test -bench=. -count=5 -benchtime=100000000x
goos: linux
goarch: amd64
pkg: github.com/robertoesteves13/aos-vs-soa
cpu: Intel(R) Core(TM) i5-9400F CPU @ 2.90GHz
BenchmarkAoS-6   	100000000	        2.396 ns/op
BenchmarkAoS-6   	100000000	        1.993 ns/op
BenchmarkAoS-6   	100000000	        1.992 ns/op
BenchmarkAoS-6   	100000000	        1.994 ns/op
BenchmarkAoS-6   	100000000	        2.084 ns/op
BenchmarkSoA-6   	100000000	        1.480 ns/op
BenchmarkSoA-6   	100000000	        1.469 ns/op
BenchmarkSoA-6   	100000000	        1.482 ns/op
BenchmarkSoA-6   	100000000	        1.485 ns/op
BenchmarkSoA-6   	100000000	        1.461 ns/op
PASS
ok  	github.com/robertoesteves13/aos-vs-soa	5.886s
```

```sh
$ go test -bench=. -count=5 -benchtime=500000000x
goos: linux
goarch: amd64
pkg: github.com/robertoesteves13/aos-vs-soa
cpu: Intel(R) Core(TM) i5-9400F CPU @ 2.90GHz
BenchmarkAoS-6   	500000000	        2.397 ns/op
BenchmarkAoS-6   	500000000	        1.995 ns/op
BenchmarkAoS-6   	500000000	        2.011 ns/op
BenchmarkAoS-6   	500000000	        1.990 ns/op
BenchmarkAoS-6   	500000000	        2.003 ns/op
BenchmarkSoA-6   	500000000	        1.495 ns/op
BenchmarkSoA-6   	500000000	        1.503 ns/op
BenchmarkSoA-6   	500000000	        1.484 ns/op
BenchmarkSoA-6   	500000000	        1.512 ns/op
BenchmarkSoA-6   	500000000	        1.487 ns/op
PASS
ok  	github.com/robertoesteves13/aos-vs-soa	29.211s
```

We can see for small sizes, it favours AoS, but for large sizes, it favours SoA.
This might be because memory locality starts to become a concern when you can't
fit all your data in cache. Also, the go compiler seems to be able to do more
optimizations when the code is AoS, but I'm not sure of that.
