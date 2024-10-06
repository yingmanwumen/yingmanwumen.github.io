---
title: Pros and Cons of Zig
date: 2024-10-02 19:40:00
excerpt: "Zig is a trending programming language, often compared to Rust and other seasoned programming languages. This article shows a quick overview of Zig and analysis its pros and cons."
---

> The primary goal of Zig is to be a better solution to the sorts of tasks that are currently solved with C.

# What is Zig? The Brief Overview

Zig is a general-purpose programming language first released on 8 February 2016 with following features: [^1] [^2] [^3] [^4] [^5]

- Imperactive programming with C-like but shrinked syntax
- Greate interaction with C
- Optional `libc` dependency
- High performance
- [Statically typed](https://en.wikipedia.org/wiki/Type_system#STATIC) and [strongly typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing)
- Explicit control flow
- Explicit manual memory allocations with no GC
- Generic types based on `comptime`
- Error handling mechanism based on [error sets](https://ziglang.org/documentation/master/#Error-Set-Type), [error union](https://ziglang.org/documentation/master/#Error-Union-Type), try-catch and so on
- [Optional types](https://ziglang.org/documentation/master/#Optionals) to handle potentially null pointer usages
- [Reflection](https://ziglang.org/documentation/master/#typeInfo)
- Strict compile-time checks
- ...

> It is designed for "robustness, optimality and maintainability".

Zig has been self-hosted since August 20, 2022.[^6]

As of October 2024, Zig still has not released a stable 1.0 version.

# More Details

## Syntax & Grammar

The syntax & grammar in Zig is **direct**, **plain**, **simple** and **explicit**, so that everything is clear at a glance.

> Zig attempts to use existing concepts and syntax wherever possible, avoiding the addition of different syntax for similar concepts.

As it is mentioned in homepage, Zig doesn't have hidden control flow.

For example, if something calls a function, it looks like a function call. A simple statement `a = b + c` doesn't look like an invocation, so it is not a function call apparently in Zig. Meanwhile, C++ and Rust allow operation overloading, so there might be an implicit function call on `+`.

In another way, the syntax & grammar in Zig is **unambiguous**.

And more over, Zig doesn't have preprocessors and macros, that's very different from C-like languages and Rust. Zig achieves the same functionalities with `comptime`. 


## Type System

Zig provides security assurance by means of its strong and static type system with strict compile-time checking.

Unlike Rust which forbids any implicit type casting, Zig allows safe and unambiguous [type coercion](https://ziglang.org/documentation/master/#Type-Coercion) for implicit type conversions.

- Implicit type casting is not allowed in Rust, even with primitive types:

```rust
let a: u8 = 1;
let b: u16 = a; // expected `u16`, found `u8`
```

- Zig support type coercion for implicit type conversions:

```zig
test "type coercion - variable declaration" {
    const a: u8 = 1;
    const b: u16 = a;
    _ = b;
}
```

Note that `_ = b;` which looks the same as the grammar in Rust is used for suppressing the unused warning.

### Error Handling With Error Types

The error handling in Zig is designed based on error types, such as error sets and error union.

The error set in Zig looks like the `enum` without payload in Rust. And the most common used is error union, which is represented by a prefix `!` before other types, such as `!i32` which means "the result of a function is either `i32` or a `error type`".

The widely used combo `try-catch` in other mordern languages is also supported in Zig.

And there is a grammar suger `errdefer` which is a variant of `defer`:

```zig
fn deferErrorCaptureExample() !void {
    errdefer |err| {
        std.debug.print("the error is {s}\n", .{@errorName(err)});
    }
    return error.DeferError;
}
```

Although the error handling in Zig is not as powerful as Rust, it is still a huge improvement compared to C.

### Null Pointers & Dangling Pointers

Null pointers are described to be [the billion dollar mistake](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare/).

Rust eliminates the null pointers as well as the dangling pointers by `Option<T>` and its unique ownership and borrowing.

Zig deals with null pointers by `?T` which is another kind of optional. Typescript has similar grammar, but Zig makes sure that a possiblely null value cannot be assigned to a variable that cannot be null.

```zig
var normal_int: i32 = 1234;
const optional_int: ?i32 = 1234;
normal_int = optional_int; // cannot convert optional to payload type
```

However, the optional types cannot avoid dangling pointers in Zig. But it improves the precision of reporting when issues do occur at runtime.

## Memory Management

There are several classical solutions to deal with memory issues:

- Garbage collection
- Automatic reference counting

Both of them are not so efficient and might need a runtime.

Rust uses complex concepts to overcome the memory issues while maintaining high runtime performance:

- Ownership
- Mutability
- Optional types
- Borrow checking
- Lifetimes
- Smart pointers
- RAII: *Resource Acquisition Is Initialization*
- ...

which cause a steep learning curve and a huge overhead in compilation time.

Zig's way to solve the memory issues is:

- Optional types
- Explicitly manual memory management via language builtin [allocators](https://ziglang.org/documentation/master/#Choosing-an-Allocator)
- Syntactic sugar: `defer`
- Strict compile-time as well as runtime checking

> Another change for memory management in Zig is that the actual allocation is handled through structs describing the action, as opposed to calling the memory management functions in libc.

Compare two versions function definitions written in C and Zig sperately:

```c
const char* repeat(const char* original, size_t times);
```

```zig
fn repeat(allocator: *std.mem.Allocator, original: []const u8, times: usize) std.mem.Allocator.Error![]const u8;
```

In C version, the memory allocation is implicit and the `malloc()` invocation is invisible, which might lead to a memory leak if the caller forgets to release the memory. A hidden allocation always causes problems.

In Zig version, the memory allocation is explicit with `*std.mem.Allocator`. Composing with `defer`, the memory leaks, double-free and use-after-free can be easily avoided... ***manually***!

Yes, as a C-like low level language, Zig provides ***limited*** protection to the programmers, and the it's the developers' responsibility to organize safety codes with the understanding of real data-layout in memory. Memory issues are **still but less possible** in Zig. This is different from Rust, which conceals the underlying details of memory layouts, instead providing a highly abstract criterions and views about memory management. As the communities say, "Zig is intended as a successor to C and Rust is to C++".

## Other Aspects

### Support for asynchronous programming

It's a pity that Zig doesn't support asynchronous programming, or rather, this feature is regressed. [^7]

### Support for functional programming

As a imperative programming language, Zig doesn't support FP (Functional Programming) well.

For example, a lambda function (or a closure) in Rust can be:

```rust
fn foo(x: i32) -> impl Fn(i32) -> i32 {
    move |y: i32| -> i32 { (x..y).filter_map(|x| (x % 2 == 0).then_some(x * x)).sum() }
}
```

And in Python, it can be:

```python
def bar(x):
    def foo(y):
        return sum([x * x for x in range(x, y) if x % 2 == 0])

    return foo
```

While in other FP languages like Haskell, it can be:

```haskell
foo x = \y -> sum . map (^2) $ filter even [x..y-1]
```

However, due to the limitations of Zig, you cannot write a closure like other languages in Zig. Instead, you need a more "explicit" way:

```zig
fn bar(x: i32) fn (i32) i32 {
    return struct {
        pub fn foo(y: i32) i32 {
            var counter: i32 = 0;
            for (x..@intCast(y)) |i| {
                if ((i % 2) == 0) {
                    const v = i * i;
                    counter += @intCast(v);
                }
            }
            return counter;
        }
    }.foo;
}
```

This is not safer than Rust or Haskell. For example, in case of $y > x$, this function call panics in Zig, while in Rust or Haskell it works correctly.

### Interaction with C

The ABI (Application Binary Interface) of Zig is similar to the ABI of C, so that Zig can interact with C natively without so-called FFI (Foreign Language Interface) or bingdings.

In another way, Zig can be used as a C compiler although it is a standalone language! (Zig itself is based on LLVM) And the `libc` dependency in Zig is optional, which is really convenient when developing embedded softwares.

### Build system & Package management

Zig provides a native build system and a package manager. [^8]

However, there is not a centralized registry in Zig, and the registry source can come from either localhost or the Internet. The common package managements for C are also acceptable in Zig.

The build system of Zig takes advantage of Zig itself with a `build.zig` script to describe a Zig project. It can also be used as a replacement for autotools like ninja.

### Ecosystem

Zig is such a young language compared to Rust (version 1.0 since 2015), Go (version 1.0 since 2012) and other widely used programing languages, no to mention that the version 1.0 of Zig is nowhere is sight. It is remains a new language, and the community and ecosystem are still in growing...

### Metaprogramming

Zig supports metaprogramming via `comptime`:

```zig
fn max(comptime T: type, a: T, b: T) T {
    return if (a > b) a else b;
}
```

`comptime` is a rather powerful concept and key feature to implement genric types in Zig.

It's more than that, a lot of optimization can be done in compilation time with the information provided by `comptime`.

### Performance

No GC, No ARC, No runtime, manual allocators with little or none overhead, optimized during compilation, `comptime`, C-compatible ABI ... All of these contributes to the high performance to Zig.

# Pros & Cons

## Pros

- **Readability and maintianability**

  The minimized concepts, straightforward syntax and explicit control flow make the language easier to read and write, especially for the developers familiar with C-like languages. All of these reduce ambiguity and greatly reduce the likelihood of making mistakes.

- **High performance**

- **Relative memory safety**

  It is much easier to maintain a bug-free program in Zig than in pure C with the boost of mordern programing concepts.

- **C-compatible ABI**

  Because of the native compatibility to C, Zig can be used as a C compiler in other scenarios, which is really useful. For instance, [cross compilation](https://dev.to/kristoff/zig-makes-go-cross-compilation-just-work-29ho).

- **Minimal dependencies**

  Even `libc` is optional in Zig, that means Zig is super portable!

- **Low-level capabilities**

  Compared to Rust, Zig exports more underlying details to programmers.

## Cons

- **Steep learning curve**

  Zig is simpler than Rust, and it is intented to be as simplified as possible. However, it is still not easy to master Zig, especially for those unfamiliar with low-level programing concepts.

- **Lake of abstract expression**

  Sometimes it might be a little bit tedious to write higher abstractions in Zig.

- **Possible memory issues**

  The memory allocator, `defer`, strict checking and optional types are wonderful. However, due to its explicitness and lake of hidden control, memory issues can still happen in runtime. It is safer than C, but not extremely safe as Rust, full of tradeoffs.

- **Immature ecosystem**

- **No language-level asynchronous programing support**

- **Lack of practice to killer application**

  Zig is self-hosted. That's nice. But there should be more killer applications to show its capabilities and scalibility.

[^1]: https://en.wikipedia.org/wiki/Zig_(programming_language)
[^2]: https://ziglang.org/
[^3]: https://en.wikipedia.org/wiki/Imperative_programming
[^4]: https://ziglang.org/documentation/master/
[^5]: https://ziglang.org/learn/why_zig_rust_d_cpp/
[^6]: https://github.com/ziglang/zig/pull/12368
[^7]: https://ziglang.org/documentation/master/#Async-Functions
[^8]: https://ziglang.org/learn/build-system/
