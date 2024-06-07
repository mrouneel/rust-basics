# Basics

Rust Syntax

### 1. Variables and Data Types
##### Scalar Types

```rust
let int_number: i32 = -42;          // signed integers: i8, i16, i32, i64, i128, isize
                                    // represent positive and negative numbers
                                    
let uint_number: u64 = 100_000;     // unsigned integers: u8, u16, u32, u64, u128, usize
                                    // represent only positive numbers

let float_number: f64 = 3.14;    // floating-point numbers: f32, f64
let is_rust_cool: bool = true;
let emoji: char = '😊'; 
```

##### Compound Types

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1); // tuple
let arr: [i32; 5] = [4, 5, 1, 2, 1]; // type: i32, length = 5
```

##### Standard Library Types

```rust
let borrowed_string = "Hello World"; // &str, a string slice

let mut owned_string = String::new(); // String
owned_string.push_str("Hello World");

let mut owned_string2 = String::from("Hello ");
owned_string2.push_str("World")

// difference: explained later with ownership

let values = vec![1, 2, 3]; // Vector (list)
let mut values2: Vec<i32> = Vec::new();

let mut numbers = vec![1, 2, 3];
```
#####     User-Defined Types


```rust
struct Person {
    name: String,
    age: u32,
}

fn main() {
    let person1 = Person {             // create an instance of the 'Person' struct and initialize all fields
        name: String::from("Alice"),
        age: 30,
    };
    println!("{} is {} old.", person1.name, person1.age);
}

enum Direction {
    Up,
    Down,
    Left,
    Right,
}
```

### 2. Conditional Statements

```rust
let number = 0;

if number > 0 && number % 2 == 0 {
    println!("The number is positive and even.");
} else if number < 0 || number % 2 != 0 {
    println!("The number is negative or odd.");
} else {
    println!("The number is zero.");
}

match number {              // with match you have to handle all cases 
    1 => println!("One!"),
    2 | 3 | 5 | 7 => println!("This is a prime number."),
    4 | 6 | 8 | 9 | 10 => println!("This is a composite number."),
    _ => println!("The number is not between 1 and 10."),
}
```
### 3. Loops

```rust
for i in 1..6 {
    println!("Number: {}", i);
}

let mut x = 1;
while x <= 5 {
    println!("Number: {}", x);
    x += 1;
}

let mut count = 0;
loop {
    println!("Hello, world!");

    count += 1;
    if count >= 5 {
        break; // Exit the loop when count reaches 5
    }
}
```

### 4. Functions

```rust
fn add(a: i32, b: i32) -> i32 {     // returns an i32 integer
    a + b                           // don't need return keyword, automatically returned
}

fn main() {
    let result = add(5, 3);
    println!("The sum is: {}", result);
}
```

### 5. Exception Handling

```rust
// with type Result
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {:?}", error),
    };
}

// macros: a way of defining reusable chunks of code; they generate code
// panic! macro
fn main() {
    if 1 + 1 != 2 {
        panic!("Math is broken!");
    }
}
```

### 6. Ownership

Set of rules that govern how a Rust program manages memory.

- Each value in Rust has an owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

```rust
{                                                    // s is not valid here, it’s not yet declared
    let s = "hello";                                // s is valid from this point forward
    let mut owned_string = String::from("Hi");
}                      // this scope is now over, and s is no longer valid, rust calls `drop`


let s1 = String::from("Hello");
let s2 = s1;                        // s2 is now the owner
s1.push_str("Wont work");          // WRONG, won't work, because s1 has been "moved" to s2 -> deals with memory

```

<div style="text-align:center;">
    <img src="https://doc.rust-lang.org/book/img/trpl04-04.svg" alt="My Image" width="400">
</div>

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);                 // provides a reference to s1, reference borrowing

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

```rust
// if wanting to modify the borrowed value
fn main() {
    let mut s = String::from("hello");

    change(&mut s);   // mutable
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```
<div style="text-align:center;">
    <img src="https://doc.rust-lang.org/book/img/trpl04-05.svg" alt="My Image" height="250">
</div>

```rust
// reference borrowing: you can have either one mutable reference (to ensure thread safety) or any number of immutable references
//                      the reference must be valid meaning the borrowed variable has to exist

let mut s = String::from("hello");      
let r1 = &mut s;
let r2 = &mut s;                    // error

// This above won't work

// This will work
let mut s = String::from("hello");

{
    let r1 = &mut s;
} // r1 goes out of scope here, so we can make a new reference with no problems.

let r2 = &mut s;


// This won't work

let mut s = String::from("hello");

let r1 = &s; // no problem
let r2 = &s; // no problem
let r3 = &mut s; // BIG PROBLEM
println!("{}, {}, and {}", r1, r2, r3);
// error -> because r3 wants to change data which r1 and r2 are reading 

// This will work
let mut s = String::from("hello");

let r1 = &s; // no problem
let r2 = &s; // no problem
println!("{} and {}", r1, r2);
// variables r1 and r2 will not be used after this point

let r3 = &mut s; // no problem
println!("{}", r3);
```



