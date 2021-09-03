
CMSC388Z Homework #1
Rust Basics
===

## Read Before You Start
1. This assignment is due on **September 10th, 2021 at noon**.
2. Please submit your code onto [**gradescope**](https://www.gradescope.com/courses/291105) electronically following the [instructions](https://help.gradescope.com/article/ccbpppziu9-student-submit-work).
3. Please make sure you are using the latest version of Rust.
    ```bash
    $ rustc --version
    rustc 1.54.0 (a178d0322 2021-07-26)
    ```
4. Please make sure your program doesn't contains any warning or error.
5. Please provide a unit test for each function you implemented.
6. Please provide a short description for each function you implemented. 
7. Please feel free to refer to any appropriate online resource.If you are not sure, you can email me (dhe17 *at* umd *dot* edu) or Chase (ckanipe *at* umd *dot* edu) for clarification.
8. This is an individual project, please do not discuss any code related questions with anybody.
9. Please fill out this survey (**placeholder**) to provide course-related feedback. 
10. We highly recommend using VS code + [rust_analyzer](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer) extention. But it is OK if you have your preferred code editor.
## Introduction

In this assignment, you will create a rust project and implement functions using the concepts we covered during class. For every function that you implement, you need to provide a unit testing function and pass it, and you also need to provide a short description to explain what you did for each function. For example,

```rust=
/// In this function, the input beetle will undergo transformation. This funtion will return a bumblebee.
pub fn transformation(beetle) -> bumblebee {
    // Transformation!
}
```

The keyword `pub` tells Rust that this function is public, so we can access to it in the `main.rs`


## 1. Create a rust project

Please create a new rust project using `cargo`. The command you need were covered in the [lecture notes](https://hackmd.io/1YbT4if2S528cRbHIMqDKg?view). Please use snake_case when naming your project. You can use `rust_hw1`, for example. 

## 2. Implementing functions
Please crete a new file `mod1.rs` under the `src` folder of your project, so the correct path of this file should be `rust_hw1/src/mod1.rs`. Then, please implement the following functions in `mod1.rs` file.  

### 2.1 adding two numbers
Implement the function that doubles an integer in three different ways:
1. A function that takes an `i32` integer as the input and returns an `i32` integer. A unit test that test the correctness of your `double_v1()`.
    ```rust=
    /// A function that double a `i32` integer. The returned value is an `i32` integer.
    pub fn double_v1(n: i32) -> i32 {
        unimplemented!()
    }
    
    #[test]
    fn test_double_v1() {
        assert_eq!(double_v1(2), 4);
        assert_eq!(double_v1(-3), -6);    
    }
    ```
2. A function that takes the inference of an `i32` integer as the input and return an `i64` integer. In this function, I want you to create a new `i32` to take the doubled value, and then shadow it by its `i64` version. A unit test that test the correctness of your `double_v2()`.
    ```rust=
    ///
    pub fn double_v2(n: &i32) -> i64 {
        unimplemented!()
    }
    ```
3. A function that double an `i32` integer in place. A unit test that test the correctness of your `double_v2()`.
    ```rust=
    ///
    pub fn double_v3(n: &mut i32) {
        unimplemented!()
    }
    ```

### 2.2 Integer square root


Implement the integer square root function`sqrt(n)` . In this function I want you to use a kind of `loops` to iterate over test some values `m` and then return the largest `m` such that $m * m <= n$. For a 'harder' version, try to do it more efficiently than trying every possibility. Remember to write unit tests here (and on all future functions).

```rust
pub fn sqrt(n: usize) -> usize {
    unimplemented!()
}
```

### 2.3 [Fibonacci number](https://en.wikipedia.org/wiki/Fibonacci_number)


Given starting fibonacci numbers n1 and n2, compute an array of length 'out_size' where `v[i]`` is the $i$-th fibonacci number. (Do not forget to write a unit test)

```rust=
pub fn fibonacci(ns: (i32, i32), out_size: usize) -> [i32] {
    unimplemented!()
}
```

### 2.4 Array

#### slice an array

In this function, I want you to first check the query `[start, end]` is valid (not out-of-boundary), then return the array slice if the query is valid.

```rust=
fn slice(arr: &[i32], range: (usize, usize)) -> Result<&[i32], String> {
    // check whether the query is valid (not out-of-boundary)
    
    // return
}

```

#### Binary search

In this function, you need to find a specific value in a sorted array and return the position of this value or nothing.

This function will take the reference of a sorted array, returns the position of the query if it is in the array or nothing if it is not in the array. The return type will be `Option<i32>`. Please write a unit test!

```rust=
pub fn binary_search(arr: &[i32], query: i32) -> Option<i32> {
    unimplemented!()
}
```

## Check your program

You should run  `cargo test`, `cargo build`, `cargo fmt`, and `cargo clippy`.

## Please submit your code onto [**gradescope**](https://www.gradescope.com/courses/291105) electronically following the [instructions](https://help.gradescope.com/article/ccbpppziu9-student-submit-work).

