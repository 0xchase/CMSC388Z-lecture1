# 09/03 Lecture 1

## Course introduction

First talk through the [website](http://www.cs.umd.edu/class/fall2021/cmsc388Z/)

There are 5 assignments and 1 final project. All of them should be submitted to gradescope.

In the first 6 weeks, we will cover the rust basics, and then we will spend 4 lectures on design patterns and some other intermediate topics, and on December, we will have a class talking aboug advanced topics and a class for project presentation (I guess thes is what project demo means).

The course content will overlap with CMSC330, but we will include new stuffs such as design patterns.

## Few survey questions

 - How many of you have taken 330? How many of you have exposure to Rust outside of 330?
 - How many of you feel you've retained at least half the content?
 - How many of you feel confident enough that you could explain the concept of ownership in Rust?

## Why Rust?

### Rust Overview

"Rust is a systems programming language that runs blazingly fast, prevents segfaults, and guarantees thread safety."

In other words: Rust is designed for speed, safety, and concurrency. 

#### What is "Systems Programming"?

Areas of systems programming:
- Operating Systems
- Device Drivers
- Embedded Systems
- Real Time Systems
- Networking
- Virtualization and Containerization
- Scientific Computing
- Video Games

Other systems languages include: C, C++, Ada, D.

Like C and C++, Rust gives developers fine control over the use of memory, and maintains a close relationship between the primitive operations of the language and those of the machines it runs on, helping developers anticipate the costs (time and space) of operations.

#### Who uses Rust?

- [Amazon](https://aws.amazon.com/blogs/aws/firecracker-lightweight-virtualization-for-serverless-computing)
- [Facebook](https://developers.libra.org/)
- [Firefox](https://servo.org)
- [Discord](https://blog.discord.com/why-discord-is-switching-from-go-to-rust-a190bbca2b1f)
- [Dropbox](https://dropbox.tech/infrastructure/rewriting-the-heart-of-our-sync-engine)
- [Coursera](https://medium.com/coursera-engineering/rust-docker-in-production-coursera-f7841d88e6ed)


"For five years running, Rust has taken the top spot as [the most loved programming language](https://insights.stackoverflow.com/survey/2020#technology-most-loved-dreaded-and-wanted-languages-loved)."

Now a [top 20 language](https://www.reddit.com/r/rust/comments/hz7dfp/rust_is_now_a_top_20_language_in_all_of_the_5/) in most language popularity rankings, and one of the fastest-growing: 
[Second most used language](https://app.powerbi.com/view?r=eyJrIjoiZTQ3OTlmNDgtYmZlMS00ZTJmLTkwYTgtMWQyMTkxNWI5NGM1IiwidCI6IjQwOTEzYjA4LTQyZTYtNGMxOS05Y2FiLTRmOWZlM2U0YzJmZCIsImMiOjl9) for Advent of Code this year, after Python: 
#### Why Rust?

- Fast
- Safe -> statically typed, and compile time checked for memory safety
- Trustworthy concurrency

In particular, Rust's goals of memory safety and trustworthy concurrency are what makes it most unique. These concepts -- in particular the issues surrounding data ownership -- can be both the most surprising and the most rewarding parts of learning Rust.

### Type Safety and Memory Safety

A language is said to be [memory-safe](https://www.zdnet.com/article/microsoft-70-percent-of-all-security-bugs-are-memory-safety-issues/) if all programs written in that language have defined semantics for all possible states.

#### Aside: [Typing](https://medium.com/android-news/magic-lies-here-statically-typed-vs-dynamically-typed-languages-d151c7f95e2b)
**Rust is a [type safe and statically typed](http://www.cs.umd.edu/class/spring2021/cmsc330/lectures/26-types.pdf) language:**

**C and C++ are not type safe!** Undefined Behavior is the root of all evil.

#### Undefined Behavior

- buffer overruns.
Your program will access elements beyond the end or before the start of an array.

```cpp    
int main() {
  int x = 1, *p = &x;
  int y = 0, *q = &y;
  *(q+1) = 5; // overwrites p
  return *p; // crash 
}

```

Other issues:
 - Has anyone taken 414?
 - Buffer overrun
 - Use after free, double free, etc
 - The most serious vulnerabilities todays are normally heap related browser vulnerabilities. 

[Leave up this xkcd](https://xkcd.com/1354/).

- Problem: C and C++ allow for direct memory dereference, no checking.

**Why then, do we not check things at runtime?**

### Performance

- **Zero Cost Abstraction:** In general, C++ amd rust implementations obey the zero-overhead principle:
  What you don???t use, you don???t pay for. And further: What you do use, you couldn???t hand code any better.

Rust performs similar (even [slightly better](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/rust-gpp.html)) than C++.


### Concurrency and data races

- No data race in rust (achieved by ownership)

### Examples of Rust Safety

No null pointer dereferences. (Not unique to Rust)

Your program will not crash because you tried to dereference a null pointer.

The problems with null
1) Used to represent a missing value: 
    `String getValue(HashTable<String, String> t)`;
3) Use to represent an error: 
    `void* malloc(size_t size)`
5) It is very easy to ignore.

```rust
// Optional Values:
enum Option<P> {
  Some(P),
  None
}
```

Still compiles down to pointer and null! But to use the value, one need to unwrap the value from `Option<P>`.

#### Handling Possible Errors

```rust
enum Result<T, E> {
  Ok(T),
  Err(E),
}

fn read(&mut fd: FileDescriptor, buf: &mut [u8]) -> Result<usize, std::io::Error>;
```

#### Memory Management
- **C and C++**: Manual memory management.
- **Python and Java**: Automatic Memory Management via a garbage collector.
    - A garbage collector traces pointers in use by the program, starting from the stack and global variables.
    - Drawback: Time critical code, performance, runtime/portability
- **Rust**: 
    - [Reference counting]() + [Non-Lexical Lifetimes](http://blog.pnkfx.org/blog/2019/06/26/breaking-news-non-lexical-lifetimes-arrives-for-everyone/#How.was.NLL.implemented)
    -  Fast, safe and smart.

## Rust install/update

To check whether you have Rust installed correctly, run this in your terminal:

```bash
$ rustc --version
```

you will see the version number

```bash
$ rustc 1.54.0 (a178d0322 2021-07-26)
```

If you have different version or you don't have rust installed: 

- If you have not installed Rust in your computer, install it through [Rust-lang website](https://www.rust-lang.org/tools/install).
- If you have it installed in your computer, update it!

    ```bash    
    $ rustup update stable
    ```

To play with Rust online in your browser, go rust-lang playground (https://play.rust-lang.org).

## Hello Cargo

Cargo is Rust???s build system and package manager. Cargo comes installed with Rust if you used the official installers  described above and in [TRPL](https://doc.rust-lang.org/book/ch01-01-installation.html#installation). Cargo is so frequently used so it has its own [book](https://doc.rust-lang.org/cargo/)! After installing Rust, You can check your Cargo version by:

```bash
$ cargo --version
```

Cargo integrated many useful commands to help you manage your projects. You can create, run, build, check and test your project with cargo's easy commands!

- Creating a Project with Cargo

```bash
$ cargo new hello_cargo
$ cd hello_cargo
```

- Building and Running a Cargo Project
```bash
$ cargo build

find target/.
```

```bash
$ cargo run
```

- Building for Release
```bash
$ cargo build --release
```

- Check the format of your code
```bash
$ cargo fmt
```

- check the syntax of your code
```bash
$ cargo clippy
```

### build a project from GitHub

```bash
$ git clone example.org/someproject
$ cd someproject
$ cargo build
```

## Hello VSCode

I suggest you to write your rust code in [VS code](https://code.visualstudio.com/) with `rust-analyzer` [extention](https://github.com/rust-analyzer/rust-analyzer). 

## Hello world!

Create a `hello_world` rust project using Cargo:

```
cargo new hello_world
cd hello_world
code .
```

You folder will contain the following files.
No `code` command on mac? [Here](https://code.visualstudio.com/docs/setup/mac) is the solution. 
![](https://i.imgur.com/drZJnCp.png)

- `src/main.rs`
![](https://i.imgur.com/ihD1LeT.png)

- `Cargo.toml`
![](https://i.imgur.com/h1GmfSI.png)

#### check fmt

- Check the format of your code
```bash
$ cargo fmt
```

If you add some empty line in the `main()`, `cargo fmt` will clean it for you.


#### clippy it!

```bash
$ cargo clippy
```

if you define an unused variable, for example 

```rust
fn main() {
    let a = 1;
    println!("Hello, world!");
}
```

`cargo clippy` will complain.


### Run it!

```bash
cargo run
```

### Build it

```bash
cargo run
```

#### Build release 
- Building for Release
```bash
$ cargo build --release
```

## Common Programming Concepts

### Variables and Mutability

In rust, variables are defined via keyword `let`. By default, variables are immutable, except you specify it.

```rust
fn main() {
  let x = 37;
  let y = x + 5;
  y
} // 42

```

```rust
fn main() {
  let x = 37;
  x = x + 5;//err
  x
}

```

```rust
fn main() {//err:
  let x:u32 = -1;
  let y = x + 5;
  y
}

```

```rust
fn main() {
  let x = 37;
  let x = x + 5;
  x
}//42

```
```rust
fn main() {
  let mut x = 37;
  x = x + 5;
  x
}//42


```
```rust
fn main() {
  let x:i16 = -1;
  let y:i16 = x+5;
  y
}//4
```

### Differences Between Variables and Constants

```rust
#![allow(unused)]
fn main() {
const MAX_POINTS: u32 = 100_000;
}

```

1. constants are defined using the `const` keyword.
2. You are not allowed to `mut` constants.
3. You **must** annotate the type of constants.
4. Constants are valid for the entire time a program runs, within the scope they were declared in.
5. constants cannot be set as the result of a functional call or any other value that could only be computed at runtime.

### Shadowing

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    let x = x * 2;

    println!("The value of x is: {}", x);
}
```
The result will be `12`.


Shadowing is useful if 
1. a value needs a few modifications in the whole program
2. we want to change the type of the value but reuse the same name.

```rust
    let spaces = "   ";
    let spaces = spaces.len();
```

but rust will complain if we do 

```rust
 let mut spaces = "   ";
    spaces = spaces.len();

```

because we want to change the value of the variable as well.



### Data Type

#### Scalar Types
A scalar type represents a single value. Rust has four primary scalar types: 

##### 1. integers
![](https://i.imgur.com/nYmQio1.png =320x)
The `isize` and `usize` types depend on the kind of computer your program is running on: 64 bits if you???re on a 64-bit architecture and 32 bits if you???re on a 32-bit architecture.


You can write integer literals in any of the forms below. Note that all number literals except the byte literal allow a type suffix, such as 57u8, and _ as a visual separator, such as 1_000.



##### 2. floating-point numbers

```rust
fn main() {
    let x = 2.0; // f64, default

    let y: f32 = 3.0; // f32
}
```

4. Booleans

```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```
The main way to use Boolean values is through conditionals, such as an if expression. 

6. characters
 
```rust
fn main() {
    let c = 'z';
    let z = '???';
    let heart_eyed_cat = '????';
}
```

Rust???s `char` type is four bytes in size and represents a Unicode Scalar Value, which means it can represent a lot more than just ASCII. Accented letters; Chinese, Japanese, and Korean characters; emoji; and zero-width spaces are all valid `char` values in Rust. Unicode Scalar Values range from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF` inclusive. However, a ???character??? isn???t really a concept in Unicode, so your human intuition for what a ???character??? is may not match up with what a char is in Rust. 


#### Compound Types

Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays. 

##### The Tuple Type


A tuple is a general way of grouping together a number of values with a variety of types into one compound type. **Tuples have a fixed length**: once declared, they cannot grow or shrink in size. Tuple will be allocated on the stack.

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

**destructuring**

In addition to destructuring through pattern matching, we can access a tuple element directly by using a period (`.`) followed by the index of the value we want to access. For example:
```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}
```

This program creates a tuple, `x`, and then makes new variables for each element by using their respective indices. As with most programming languages, the first index in a tuple is 0.


##### The Array Type

Another way to have a collection of multiple values is with an array. Unlike a tuple, every element of an array **must have the same type**. Arrays in Rust are different from arrays in some other languages because arrays in Rust have a **fixed length**, like tuples.

1. Creating an array
    ```rust
    fn main() {
        let a: [i32; 5] = [1, 2, 3, 4, 5];
    }
    ```

    Here, `i32` is the type of each element. After the semicolon, the number `5` indicates the array contains five elements.

    ```rust
        let a = [3; 5];
    ```

    The array named `a` will contain `5` elements that will all be set to the value `3` initially. This is the same as writing `let a = [3, 3, 3, 3, 3]`; but in a more concise way.

2. Accessing array elements
    An array is a single chunk of memory allocated on the stack. You can access elements of an array using indexing, like this:
    ```rust
    pub fn array() {
        // Size is hardcoded for arrays.
        // There space is allocated directly in the binary.
        let a = [1, 2, 3];
        let zeroes = [0; 1000];
        let b: [i32; 3] = [1, 2, 3];

        // Typical example
        let input_files = ["input/input_1.txt", "input/input_2.txt"];

        let first = a[0];
        let second = a[1];
        // println!("{:?}", a + a);
        // a.push(3);
    }
    ```

3. Buffer overflow?
    **NEVER**. Rust checks the bounds at both compile time and run time. You will get an OOB error.


#### Statements and Expressions
- Statements are instructions that perform some action and **do not return a value. **
- Expressions evaluate to a resulting value. 


(copy/paste this)
```rust
// function body
fn main() {
    let x = 5;

    let y = {
        let x = 3;
        x + 1 
    }; // statement

    println!("The value of y is: {}", y);
}
```

```rust
// function body
fn main() {
    let x = 5; // statement

    let y = {
        let x = 3; // statement
        x + 1      // expression
    }; // statement

    println!("The value of y is: {}", y); // statement
}
```

In the function body, this block is an expression.

```rust
{
    let x = 3; // statement
    x + 1      // expression
}
```

### Comments

In each line, everything after a double reversed slash `//` can be used to mark comments. 

```rust
 // I am a comment.
a  = 5; // I am also a comment.

```


Before a function definition, a triple reversed slash `///` can be used to mark the description of a function.

```rust
/// I am the description of function `a_function()`.
fn a_function() {}
```

### Control Flow

#### `if`

```rust
fn main() {
    let number = 6;

    let sign = if number < 0 {
        -1
    } else if number == 0 {
        0
    } else {
        1
    };
}
```

#### loops

##### `loop`

The loop keyword tells Rust to execute a block of code over and over again forever or until you explicitly tell it to stop.

```rust
fn main() {
    loop {
        println!("again!");
    }
}
```

##### `while`


```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{}!", number);
        number -= 1;
    }
    
    println!("LIFTOFF!!!");
}
```


##### `for`

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    let mut index = 0;

    while index < 5 {
        println!("the value is: {}", a[index]);

        index += 1;
    }
}
```

Rust provides a way to **iterate** over a collection

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```

If you know exactly which elements you want to iterate, used `Range`:

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    for element in (1..4).rev() {
        println!("the value is: {}", element);
    }
}
```

## Testing


In any language, there is the need to test code.

In most languages, testing requires extra libraries:
- Minitest in Ruby
- Ounit in Ocaml
- Junit in Java

Testing in Rust is a first-class citizen! 
The testing framework is **built into cargo**. 

### Unit testing

Unit testing is for local or private functions
Put such tests in the same file as your code
- Use `assert!` to test that something is true
- -Use `assert_eq!` to test that two things that implement the PartialEq trait are equal. 
    - E.g., integers, booleans, etc.


```rust
fn main() {
    println!("{}", bad_add(1, 2));
}

fn bad_add(a: i32, b: i32) -> i32 {
    a - b
}

#[test]
fn test_bad_add () {
    assert_eq!(bad_add(1,3),3);
}


```


### Integration testing

Integration testing is for APIs and whole programs
 (This is how we grade your projects).

- Create a tests directory
- Create different files for testing major functionality
- Files don???t need  #[cfg(test)] or a special module
    - But they do still need #[test] around each function
- Tests refer to code as if it were an external library
    - Declare it as an external library using extern crate
    - Include the functionality you want to test with use

`src/lib.rs`
```rust
pub fn add(a: i32, b: i32) -> {
    a + b
}
```

`tests/test_add.rs`
```rust
extern crate your_project_name // this will tell rust you have an external source
use your_project_name::add; // this will tell rust you will use add() function from the extern source

#[test]
pub fn test_add () {
    assert_eq!(add(1,2),3);
}

#[test] 
pub fn test_negative_add() {
    assert_eq!(add(1,-2),-1);
}
```

# 09/10

#### vector

```rust
    // Instead, Vectors are dynamically allocated.
    // Talk about heap and stack, and "pointers".
    let mut v: Vec<i32> = 
    let mut v: Vec<i32> = vec![1, 2, 3]; // a macro to allocate vector
    println!("{:?}", v);
    v.push(3);
    println!("{:?}", v);
    v.pop();
    println!("{:?}", v);
    v.push(5);
    println!("{:?}", v);
    v.push(6);
    println!("{:?}", v);

    println!("{:?}", v.len());


    // Rust vectors have their capacity and length fields
    // as part of the vector size, accessing elements requires
    // following the pointer down to the heap.
    
}
```

#### slice

We can take slice of vector, array and string. 

Slice is a portion of some contiguous chunk of memory. 

```rust
pub fn slices() {
    let a = [1, 2, 3];
    // Fat pointer: pointer to heap, length, capacity,
    let v = vec![1, 2, 3];

    // Take a reference to the address of the vector.
    // We call this a _slice_, notice the type has changed to
    // resemble an array.

    // Arrays and Vector references kinda have the same memory layout...
    let v_slice: &[i32] = &v;
    let a_slice: &[i32] = &a;
    // So we can take references to both! (There is some automatic conversion from
    // &Vec<i32> to &[i32])...

    // How big are references?
    // Probably the size a pointer.

    // Accept a vector:
    // Write functions that can take either.
    fn print(s: &[i32]) {
        println!("{:?}", s);
    }

    // Same function prints both arrays and vector references.
    print(v_slice);
    print(a_slice);
    // Rust knows how to convert vectors to slices.
    print(&v);
    print(&v[1..2]);
}
```


#### Strings
Handling of strings is a major security risk!

Examples: https://owasp.org/www-project-top-ten/
Why are strings so hard to get right?

Most common string types in rust are String, and str.
 - String is the dynamic heap string type, like Vec: use it when you need to own or modify your string data.
 - str is an immutable1 sequence of UTF-8 bytes of dynamic length somewhere in memory. Since the size is unknown, one can only handle it behind a pointer. This means that str most commonly2 appears as &str: a reference to some UTF-8 data, normally called a "string slice" or just a "slice". A slice is just a view onto some data, and that data can be anywhere.


![](https://doc.rust-lang.org/book/img/trpl04-06.svg =320x)
```rust
pub fn strings() {
    let s1: &str = "This is a string";
    let s2: &str = "This is a string with spaghetti ????";

    // Uh oh...
    let s3: &str = "This is also a s??????????????????????????????????????tring";

    println!("{}", s1);
    println!("{}", s2);
    println!("{}", s3);
    println!("{} {} {}", 
            s1.len(), 
            s2.len(), 
            s3.len()
    ); // 16 36 59
```
The problem is, the string we see is not the way they are stored in Rust. Rust strores each string as  a `Vec<u8>`. If the string encodes only UTF-8 characters, each byte corresponds to a character, like `s1`. However, if the string contains non-UTF-8 characters, such as unicode characters, for example `s2` and `s3`, as you can see, the length of the strings are not equalt to the number of characters we see in the string. 
```

### Functions

1. `fn` keyword allows you to define function.
2. snakecase is the convention of naming functions in rust.

```rust
fn main() {
    let name = give_me_a_name();
    who_is_good(name);
}

fn give_me_a_name() -> String {
    String::from("Zaza")
}
fn who_is_good(name: String) {
    println!("{} is good.", name);
}
```
