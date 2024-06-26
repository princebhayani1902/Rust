# Rust

- learning rust from the basics.
- It covers concepts like memory management, ownership, borrowing, structures, data types, enums, and much more.

---

## 01 Preface
### Phrase #1
- In Rust, if you program compiles, it probably works.

### Phrase #2
- You can't segfault if you don't have null.

### Phrase #3
- Rust doesn't hide complexity from developers it offers them the right tools to manage all the complexity.

---

## 02 Why rust? Isn’t Node.js enough?
### Type safety
### Systems language
It is intended to be used (but not restricted to) to do lower level things
1. Building a Compiler
2. Building a browser
3. Working closer to the OS/kernel

### Generally faster
- Rust has a separate compilation step (similar to C++) that spits out an optimized binary and does a lot of static analysis at compile time. 
- JS does JIT compilation. 

![](/02%20Why%20Rust/pic1.png)
![](/02%20Why%20Rust/pic%202.png)

### Concurrency
- Rust has built-in support for concurrent programming allowing multiple threads to perform tasks simultaneously without risking data races.
- Javascript is single threaded generally (there are some projects that tried making it multi threaded but rarely used).

### Memory safe
- Rust has a concept of owners, borrowing and lifetimes that make it extremely memory safe.

> [!IMPORTANT]
> Rust doesn't hide complexity from developers it offers them the right tools to manage all the complexity.

---

- Rust projects can be used to do a lot of things
    1. Create Backend for a Full stack app
    2. Create CLIs (command line interfaces)
    3. Create browsers
    4. Great Code Editors

- For running rust locally, you should be able to run ```cargo``` in your terminal.
> [!NOTE]
> Cargo is similar to ```npm```. It’s a package manager for rust.

---
#### 03 Initializing a rust project
- Run the following command to bootstrap a simple Rust project

```Rust
mkdir rust-project
cd rust-project
cargo init
```
---

## 04 Simple Variables in Rust
- You can define variables using the ```let``` keyword (very similar to JS)
- You can assign the type of the variable, or it can be inferred as well.
#### 1. Numbers
```Rust
fn main() {
    let x: i32 = 1;
    println!("{}", x);
}
```
- Here, i32 shows size(in bits) of variable.
- i8, i16, i32, i64 etc for signed integer and u8,u16, u32, u64 is for unsigned integer.
- for float f8, f16,f32 etc.
- by default it will i32. also if you not assign type it will assign by default.

> [!CAUTION]
> What happens if we overflow?
```Rust
fn main() {
    let mut num: i8 = 124;
    for i in 0..100 {
        num += 127;
    }
    print!("Number: {}", num)
}
```
output:
> attempt to add with overflow

#### 2. Booleans
- Booleans can have two states, true or false.
```Rust
fn main() {
    let is_male = false;
    let is_above_18 = true;
    
    if is_male {
        println!("You are a male");

    } else {
        println!("You are not a male");
    }

    if is_male && is_above_18 {
        print!("You are a legal male");
    }
}
```

#### 3. String
- There are two ways of doing ```strings``` in rust. We’ll be focussing on the easier one.
```Rust
fn main() {
    let greeting = String::from("hello world");
    println!("{}", greeting);

    // print!("{}", greeting.chars().nth(1000))
}
```
---

## 05 Conditionals, loops
#### Conditionals
```Rust
pub fn main() {
    let x = 99;
    let is_even = is_even(x);
    if is_even {
        print!("{} is even", x);
    } else {
        print!("{} is odd", x);
    }
}

pub fn is_even(x: i32) -> bool {
    return x % 2 == 0;
}
```

#### Loops
```Rust
pub fn main() {
    let str = String::from("Prince Bhayani");
    println!("First name {}", get_first_name(str))
    
}

pub fn get_first_name(str: String) -> String {
    let mut first_name = String::from("");
    for c in str.chars() {
        if c == ' ' {
            break
        }
        first_name.push(c);
    }
    return first_name;
}
```
---

## 06 Functions
```Rust
fn do_sum(a: i32, b: i32) -> i32 {
	return a + b;
}
```
---

## 07 Memory Management in Rust
![](./07%20Memory%20Managment%20in%20Rust/pic1.png)
- Whenever you run a program (C++, Rust, JS), it ```allocates ```and ```deallocates``` memory on the RAM.

For example, for the following JS code:
```Rust
function main() {
  runLoop();
}

function runLoop() {
  let x = [];
  for (let i = 0; i < 100000; i++) {
    x.push(1);
  }
  console.log(x);
}

main();
```
> as the ```runLoop``` function runs, a new array is pushed to RAM, and eventually ```garbage collected```

There are 3 popular ways of doing memory management.
![](./07%20Memory%20Managment%20in%20Rust/pic2.png)

- Memory management is a crucial aspect of programming in Rust, designed to ensure safety and efficiency without the need for a garbage collector. 
- Not having a ```garbage collector``` is one of the key reasons rust is so fast
- It achieves this using the
    1. Mutability
    2. Heap and memory
    3. Ownership model
    4. Borrowing and references
    5. Lifetimes

---

## 08 Jargon #1 - Mutability
#### Mutability
Immutable ```variables``` represent variables whose value cant be changed once assigned.
```Rust
fn main() {
    let x: i32 = 1;
    x = 2; // Error because x is immutable
    println!("{}", x);
}
```
> [!NOTE]
> By default, all variables in Rust are immutable.

1. Immutable data is inherently thread-safe because if no thread can alter the data, then no synchronization is needed when data is accessed concurrently.
2. Knowing that certain data will not change allows the compiler to optimize code better. 

- You can make variables mutable by using the ```mut``` keyword.
```Rust
fn main() {
    let mut x: i32 = 1;
    x = 2; // No error
    println!("{}", x);
}
```
> const in Javascript is not the same as immutable variables in rust. In JS, you can still update the contents of const ```arrays``` and ```objects ```.

---

## 09 Jargon #2 - Stack vs Heap
#### Stack vs. Heap Allocation
Rust has clear rules about stack and heap data management:

- Stack: Fast allocation and deallocation. Rust uses the stack for most primitive data types and for data where the size is known at compile time (eg: numbers).
- Heap: Used for data that can grow at runtime, such as vectors or strings.

#### What’s stored on the stack?
1. Numbers - i32, i64, f64 …
2. Booleans - true, false
3. Fixed sized arrays 

#### What’s stored on heap?
1. Strings
2. Vectors 

#### Examples
- Hello world with numbers
![](./09%20Jargon%202%20-%20Stack%20vs%20Heap/pic1.png)

- Hello world with functions
![](./09%20Jargon%202%20-%20Stack%20vs%20Heap/pic2.png)
![](./09%20Jargon%202%20-%20Stack%20vs%20Heap/pic3.png)
![](./09%20Jargon%202%20-%20Stack%20vs%20Heap/pic4.png)

- Hello world with strings
![](./09%20Jargon%202%20-%20Stack%20vs%20Heap/pic5.png)

#### Memory in action
```Rust
fn main() {
    stack_fn();   // Call the function that uses stack memory
    heap_fn();    // Call the function that uses heap memory
    update_string();  // Call the function that changes size of variable at runtime
}

fn stack_fn() {
    // Declare a few integers on the stack
    let a = 10;
    let b = 20;
    let c = a + b;
    println!("Stack function: The sum of {} and {} is {}", a, b, c);
}

fn heap_fn() {
    // Create a string, which is allocated on the heap
    let s1 = String::from("Hello");
    let s2 = String::from("World");
    let combined = format!("{} {}", s1, s2);
    println!("Heap function: Combined string is '{}'", combined);
}

fn update_string() {
    // Start with a base string on the heap
    let mut s = String::from("Initial string");
    println!("Before update: {}", s);
    println!("Capacity: {}, Length: {}, pointer: {:p}",s.capacity(),s.len(),s.as_ptr());

    // Append some text to the string
    s.push_str(" and some additional text");
    println!("After update: {}", s);
    println!("Capacity: {}, Length: {}, pointer: {:p}",s.capacity(),s.len(),s.as_ptr());
}
```

Output:
```
Stack function: The sum of 10 and 20 is 30
Heap function: Combined string is 'Hello World'
Before update: Initial string
Capacity: 14, Length: 14, pointer: 0x1cc97809940
After update: Initial string and some additional text
Capacity: 39, Length: 39, pointer: 0x1cc97802b90
```

Memory stack and heap:
![](./09%20Jargon%202%20-%20Stack%20vs%20Heap/pic61.png)
![](./09%20Jargon%202%20-%20Stack%20vs%20Heap/pic62.png)

---

## 10 Jargon #3 - Ownership
![](./10%20Jargon%203%20-%20Ownership/pic1.png)

#### Stack variables
Example #1 - Passing stack Variables inside functions
```Rust
fn main() {
		let x = 1; // crated on stack
		let y = 3; // created on stack
    println!("{}", sum(x, y));
    println!("Hello, world!");
}

fn sum(a: i32, b: i32) -> i32 {
    let c = a + b;
    return c;
}
```
- This might sound trivial since if the function is popped of the stack, all variables go away with it, but check the next example.

Example #2 - Scoping variables in the same fn
```Rust
fn main() {
    let x = 1; // crated on stack
    {
        let y = 3; // created on stack
    }

    println!("{}", y); // throws error
}
```

#### Heap variables
- Heap variables always want to have a single owner, and if their owner goes out of scope, they get de-allocated.

- Any time the owner of a heap variable goes out of scope, the value is de-allocated from the heap.

Example #1 - Passing strings (heap variables) to functions as args
```Rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;
    println!("{}", s1); // This line would cause a compile error because ownership has been moved.
}
```
![](./10%20Jargon%203%20-%20Ownership/pic2.svg)

Another example of the same thing - 
```Rust
fn main() {
    let my_string = String::from("hello");
    takes_ownership(my_string);
    println!("{}", my_string); // This line would cause a compile error because ownership has been moved.
}

fn takes_ownership(some_string: String) {
    println!("{}", some_string); // `some_string` now owns the data.
}
```

- At any time, each value can have a single owner. This is to avoid memory issues like.
    1. Double free error.
    2. Dangling pointers.

#### Fix?
Clone the string
```Rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();
    println!("{}", s1); // Compiles now
}
```
![](./10%20Jargon%203%20-%20Ownership/pic3.png)

But what if you want to pass the same string over to the function? You don’t want to clone it, and you want to return back ownership to the original function?

You can either do the following -
```Rust
fn main() {
    let s1 = String::from("hello");
    let s2 = takes_ownership(s1);
    println!("{}", s2);
}

fn takes_ownership(some_string: String) -> String {
    println!("{}", some_string); 
    return some_string; // return the string ownership back to the original main fn
}
```
You can also do this
```Rust
fn main() {
    let mut s1 = String::from("hello");
    s1 = takes_ownership(s1);
    println!("{}", s2);
}

fn takes_ownership(some_string: String) -> String {
    println!("{}", some_string); 
    return some_string; // return the string ownership back to the original main fn
}
```

> [!NOTE]
> Is there a better way to pass strings (or generally heap related data structures) to a function without passing over the ownership?
>
> Yes - References

---

## 11 Jargon #4 - Borrowing and References
#### References
References mean giving the address of a string rather than the ownership of the string over to a function

For example
```Rust
fn main() {
    let s1 = String::from("Hello");
    let s2 = &s1;

    println!("{}", s2);
    println!("{}", s1);    // This is valid, The first pointer wasn't invalidated
}
```
![](./11%20Jargon%204%20-%20Borrowing%20and%20References/pic1.png)

#### Borrowing
You can transferring ownership of variables to fns. By passing a reference to the string to the function ```borrowing_ownership```, the ownership of the string remains with the original variable, in the ```main``` function. This allows you to use ```my_string``` again after the function call.

```Rust
fn main() {
    let my_string = String::from("Hello, Rust!");
    borrowing_ownership(&my_string);  // Pass a reference to my_string
    println!("{}", my_string);    // This is valid because ownership was not transferred
}

fn borrowing_ownership(some_string: &String) {
    println!("{}", some_string);  // some_string is borrowed and not moved
}
```

#### Mutable references
What if you want a function to ```update``` the value of a variable?
```Rust
fn main() {
    let mut s1 = String::from("Hello");
    update_word(&mut s1);
    println!("{}", s1);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
```

Try having more than one mutable reference at the same time - 
```Rust
fn main() {
    let mut s1 = String::from("Hello");
    let s2 = &mut s1;
    update_word(&mut s1);
    println!("{}", s1);
    println!("{}", s2);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
```
- This gives an Error.

#### Rules of borrowing
- There can me many ```immutable references``` at the same time
```Rust
fn main() {
    let  s1 = String::from("Hello");
    let s2 = &s1;
    let s3 = &s1;
    
    println!("{}", s1);
    println!("{}", s2);
    println!("{}", s3);
}
// No errors
```

- There can be only one ```mutable reference```  at a time
```Rust
fn main() {
    let mut s1 = String::from("Hello");
    let s2 = &mut s1;
    let s3 = update_word(&mut s1);
    
    println!("{}", s1);
    println!("{}", s2);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
// Error
```

- If there is a ```mutable reference``` , you can’t have another immutable reference either.
```Rust
fn main() {
    let mut s1 = String::from("Hello");
    let s2 = &mut s1;
    let s3 = &s1;
    
    println!("{}", s1);
    println!("{}", s2);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
```

This to avoid any data races/inconsistent behaviour.

- If someone makes an ```immutable reference```, they don’t expect the value to change suddenly.
- If more than one ```mutable references``` happen, there is a possibility of a data race and synchronization issues.

---

## 12 Structs
Structs in rust let you structure data together. Similar to ```objects``` in javascript.
```Rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };
    print!("User 1 username: {:?}", user1.username);
}
```

Can you guess if they are stored in ```stack``` or ```heap```?
![](./12%20Structs/pic1.png)


#### traits
1. ```mutable``` and ```immutable``` references
2. Ownership transfer

Code 1 - Only stack types in the struct
```Rust
struct User {
    active: bool,
    sign_in_count: u64,
}

fn main() {
    let mut user1 = User {
        active: true,
        sign_in_count: 1,
    };

    print_name(user1);
    print!("User 1 username: {}", user1.active); // Error - can not use borrowed value
}

fn print_name(user1: User) {
    print!("User 1 username: {}", user1.active);
}
```
- gives an Error 

Now Add the ```copy trait```
```Rust
#[derive(Copy, Clone)]
struct User {
    active: bool,
    sign_in_count: u64,
}

fn main() {
    let mut user1 = User {
        active: true,
        sign_in_count: 1,
    };

    print_name(user1);
    print!("User 1 username: {}", user1.active); // Error goes away because user1 is copied
}

fn print_name(user1: User) {
    print!("User 1 username: {}", user1.active);
}
```

Code 2 - Add strings 
```Rust
struct User {
    active: bool,
    sign_in_count: u64,
    username: String,
}

fn main() {
    let mut user1 = User {
        active: true,
        sign_in_count: 1,
        username: "harkirat".to_string()
    };

    change_name(user1);
    print!("User 1 username: {}", user1.active); // Error - can not use borrowed value
}

fn change_name(user1: User) {
    print!("User 1 username: {:?}", user1.active);
}
```
- gives an Error 

Try adding the ```Copy trait``` (you wont be able to because strings don't implement them, use ```clone``` trait instead)
```Rust
#[derive(Clone)]
struct User {
    active: bool,
    sign_in_count: u64,
    username: String,
}

fn main() {
    let mut user1 = User {
        active: true,
        sign_in_count: 1,
        username: "harkirat".to_string()
    };

    change_name(user1.clone());
    print!("User 1 username: {}", user1.active); // Error - can not use borrowed value
}

fn change_name(user1: User) {
    print!("User 1 username: {:?}", user1.active);
}
```

---

## 13 Implementing structs
You can also ```implement structs``` , which means you can attach functions to instances of structs
(Very similar to classes in TS)
```Rust
struct Rect {
   width: u32,
   height: u32,
}

impl Rect {
    fn area(&self) -> u32 {
         self.width * self.height
    }
}

fn main() {
    let rect = Rect {
        width: 30,
        height: 50,
    };
    print!("The area of the rectangle is {}", rect.area());
}
```

---

## 14 Enums
Enums in rust are similar to enums in Typescript. They allow you to define a type by enumerating its possible variants.

```Rust
enum Direction {
    North,
    East,
    South,
    West,
}

fn main() {
    let my_direction = Direction::North;
    let new_direction = my_direction; // No error, because Direction is Copy
    move_around(new_direction);
}

fn move_around(direction: Direction) {
    // implements logic to move a character around
}
```

Why not simply do the following -
```Rust
fn main() {
    move_around("north".to_string());
}

fn move_around(direction: String) {
    if direction == "north" {
        println!("Moving North");
    }
}
```
Because we don’t enforce the 4 variants of directions. So this is much looser than strictly allowing only 4 variants for direction.

#### Enums with values
```Rust
// Define an enum called Shape
enum Shape {
    Circle(f64),  // Variant with associated data (radius)
    Square(f64),  // Variant with associated data (side length)
    Rectangle(f64, f64),  // Variant with associated data (width, height)
}

// Function to calculate area based on the shape
fn calculate_area(shape: Shape) -> f64 {
    // calculates the area of the shape 
    return 0
}

fn main() {
    // Create instances of different shapes
    let circle = Shape::Circle(5.0);
    let square = Shape::Square(4.0);
    let rectangle = Shape::Rectangle(3.0, 6.0);
    
}
```

---

## 15 Pattern matching
Let you pattern match across various variants of an enum and run some logic.
```Rust
// Define an enum called Shape
enum Shape {
    Circle(f64),  // Variant with associated data (radius)
    Square(f64),  // Variant with associated data (side length)
    Rectangle(f64, f64),  // Variant with associated data (width, height)
}

// Function to calculate area based on the shape
fn calculate_area(shape: Shape) -> f64 {
    match shape {
        Shape::Circle(radius) => std::f64::consts::PI * radius * radius,
        Shape::Square(side_length) => side_length * side_length,
        Shape::Rectangle(width, height) => width * height,
    }
}

fn main() {
    // Create instances of different shapes
    let circle = Shape::Circle(5.0);
    let square = Shape::Square(4.0);
    let rectangle = Shape::Rectangle(3.0, 6.0);

    // Calculate and print the areas
    println!("Area of circle: {}", calculate_area(circle));
    println!("Area of square: {}", calculate_area(square));
    println!("Area of rectangle: {}", calculate_area(rectangle));
}
```

---

## 16 Error handling
Different languages have different ways to handle errors. 

Javascript, for example, has the concept of try catch blocks
```js
try {
    const data = fs.readFileSync('example.txt', 'utf8');
    console.log("File content:", data);
} catch (err) {
    console.error("Error reading the file:", err);
}
```
The reason we put the code inside a try catch block is that ```reading a file``` is ```unpredictable```. 

The file might not exist, the file might be locked by another process, and hence there is a possibility of this code ```throwing an error```.

The same is true for a rust program trying to access a file. But the way rust does error handling is slightly different.

#### Result Enum
```Rust 
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```
If you look at the code above, it is an enum (with generic types)

This enum is what a function can return/returns when it has a possibility of throwing an error

For example
```Rust
use std::fs::File;

fn main() {
	let greeting_file_result = fs::read_to_string("hello.txt");
}
```
Notice the type of ```greeting_file_result``` in VSCode

It returns an enum that looks as follows. It’s an enum with the ```Ok``` variant having a string value and ```Err```variant having an Error value 
```Rust
enum Result{
    Ok(String),
    Err(Error),
}
```

Complete code
```Rust
use std::fs;

fn main() {
    let greeting_file_result = fs::read_to_string("hello.txt");

    match greeting_file_result {
        Ok(file_content) => {
            println!("File read successfully: {:?}", file_content);
        },
        Err(error) => {
            println!("Failed to read file: {:?}", error);
        }
    }
}
```
Incase you write a function yourself, you can also return a ```Result``` from it. As the name suggests, ```Result``` holds the ```result``` of a function call that might lead to an error. 

#### Unwraps
Incase you are ok with runtime errors (crashing the process while it runs if an error happens), then you can ```unwrap``` a ```Result```
```Rust
use std::fs;

fn main() {
    let greeting_file_result = fs::read_to_string("hello.txt");
    print!("{}", greeting_file_result.unwrap());
}
```

#### Returning a custom error
```Rust
use core::fmt;
use std::{fmt::{Debug, Formatter}, fs};

pub struct FileReadError {

}

fn main() {
    let contents = read_file("hello.txt".to_string());
    match contents {
        Ok(file_content) => {
            println!("File content: {}", file_content);
        },
        Err(error) => {
            println!("Error reading file: {:?}", error);
        }
    }
}

fn read_file(file_path: String) -> Result<String, FileReadError> {
    let greeting_file_result = fs::read_to_string("hello.txt");
    match greeting_file_result {
        Ok(file_content) => {
            Ok(file_content)
        },
        Err(error) => {
            let err = FileReadError {};
            Err(err)
        }
    }
}
```

---

## 17 Option enum
The Option enum was introduced in Rust to handle the concept of nullability in a safe and expressive way. Unlike many programming languages that use a null or similar keyword to represent the absence of a value, Rust doesn't have null.
```Rust
pub enum Option<T> {
    None,
    Some(T),
}
```

If you ever have a function that should return null, return an ```Option``` instead.

For example
```Rust
fn find_first_a(s: String) -> Option<i32> {
    for (index, character) in s.chars().enumerate() {
        if character == 'a' {
            return Some(index as i32);
        }
    }
    return None;
}

fn main() {
    let my_string = String::from("raman");
    match find_first_a(my_string) {
        Some(index) => println!("The letter 'a' is found at index: {}", index),
        None => println!("The letter 'a' is not found in the string."),
    }
}
```

---

## 18 Cargo, packages and external deps
Just like the ```nodejs``` ecosystem has ```npm```, the rust ecosystem has ```cargo```.

Cargo is a ```package manager``` for rust, which means we can use it to bring packages (crates in case of rust) to our projects


#### Generate a random number
Run ```cargo add rand```
```Rust
use rand::{Rng, thread_rng};

fn main() {
    let mut rng = thread_rng();
    let n: u32 = rng.gen();
    println!("Random number: {}", n);
}
```
Output:

> Random number: 2497579865


#### Store time in a DB/as a variable
Run ```cargo add chrono```
```Rust
use chrono::{Local, Utc};

fn main() {
    // Get the current date and time in UTC
    let now = Utc::now();
    println!("Current date and time in UTC: {}", now);

    // Format the date and time
    let formatted = now.format("%Y-%m-%d %H:%M:%S");
    println!("Formatted date and time: {}", formatted);

    // Get local time
    let local = Local::now();
    println!("Current date and time in local: {}", local);
}
```
Output:

>Current date and time in UTC: 2024-05-03 18:23:20.777067900 UTC
>
>Formatted date and time: 2024-05-03 18:23:20
>
>Current date and time in local: 2024-05-03 23:53:20.777855200 +05:30

---