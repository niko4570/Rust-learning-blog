# Ownership in Rust

## Introduction

Ownership is a unique feature of Rust that enables memory **safety** without a garbage collector. It is based on a set of rules that govern how memory is managed in Rust programs. Understanding ownership is crucial for writing efficient and safe Rust code.

## Rules of Ownership

1. **Each value has an owner.**

```rust
    let s = String::from("hello");
    // s is the owner of the String value

```

2. **There can only be one owner at a time.**

- In this example, when `s` is assigned to `s1`, the ownership of the String value is moved from `s` to `s1`. Similarly, when `s1` is assigned to `s2`, the ownership is moved from `s1` to `s2`. After each move, the previous owner is no longer valid and cannot be used.

```rust
    let s = String::from("hello");//s is the owner of the String value
    let s1 = s;// the owner is moved from s to s1, s is no longer valid
    let s2 = s1; // s1 is moved to s2, s1 is no longer valid
    // println!("{}", s1);This would cause a compile-time error,because the owner of the String value has changed from s1 to s2. so s1 is no longer valid.
```

3. **When the owner goes out of scope, the value will be dropped.**

- In this example, when `s` is dropped, the memory allocated for the String value is freed, preventing memory leaks and ensuring memory safety.

```rust
let s = String::from("hello");
    // s is the owner of the String value
{
    s// `s` is dropped here when it goes out of scope
}
//println!("{s}"); //This can't be compiled because s is out of scope.

```

In the examples above, all variables are of type `String`, which stores data on the `heap`. Values stored on the `heap` strictly follow the ownership rules mentioned above.

However, Rust also has types that are stored on the `stack`, such as `i32`, `bool`, and `char`. These types implement the `Copy` trait, which means they are copied rather than moved:

```rust
let x = 5;      // i32 stored on stack
let y = x;      // Value is copied, both x and y are valid

println!("x = {}, y = {}", x, y);  // This works fine!
```

So, we can see:: **values stored on the `heap` follow the above rules, values stored on the `stack` will `copy`**

Click here to learn [Heap AND Stack](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html) in Rust.

## Borrowing with References

What if we need to use a variable multiple times without transferring ownership?
We use borrowing with references `&`:

1. **Using References**

```rust

let s = String::new("Hello");

{
    println!("In a block:{}",&s);// Borrow s as immutable reference
}

println!("{}",s)//s is still valid

```

2. **Multiple Immutable References**

```rust

let s = String::from("hello");

// Multiple immutable references are allowed
let r1 = &s;
let r2 = &s;

println!("r1: {}, r2: {}", r1, r2);  // Both references can be used

```

3. **Mutable References**

```rust
let mut s = String::from("hello");

{
    let s_ref = &mut s;
    s_ref.push_str(", world!");
} // s_ref goes out of scope here

println!("{}", s);  // Prints "hello, world!"
```

4. **Function Parameters with References**

```rust
fn calculate_length(s: &String) -> usize {
    s.len()
} // s goes out of scope, but since it's a reference, nothing happens

let s = String::from("hello");
let len = calculate_length(&s);
println!("The length of '{}' is {}.", s, len);  // s is still valid
```

5. **Mutable References in Loops**

```rust
let mut s = String::from("hello");

for i in 0..3 {
    let s_ref = &mut s;
    s_ref.push_str("!");
    println!("Iteration {}: {}", i, s_ref);
}

println!("Final result: {}", s);  // Prints "hello!!!"
```

6. **Slices**

```rust
let s = String::from("hello world");
let hello = &s[0..5];
let world = &s[6..11];

println!("{} {}", hello, world);  // Prints "hello world"
```

- using `&` to create immutable referance (borrowing)
- Use `&mut` to create mutable references (exclusive borrowing)
- You can have **multiple** immutable references OR **one** mutable reference, **but not both at the same time**
- Types that implement `Copy` trait are copied automatically

## Reference

- [Understanding Ownership](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html)

- [Slice](https://doc.rust-lang.org/book/ch04-03-slices.html)
