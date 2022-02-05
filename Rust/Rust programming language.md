# Rust programming language

#### Ownership and Borrowing

Ownership rules in rust

- Each value in Rust has a variable that's calls its owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.



Example

```rust
{
    // This string s will be allocated in the heap
    let s = String::from("Hello");
    // Do some stuff here with s
    // Compiler will drop s after this scope
}


```

Pointer examples

```rust
let x = 5;
let y = x; // Copy


let s1 = String::from("Hello");
let s2 = s1; // Move pointer to s2 and s1 will be no longer valid
println!("{}", s1); // This will call an error. s1 no longer valid

```

Ownership example

```rust
fn main(){
    let s = String:from("Hello");
    take_ownership(s);

    // Error s can't be boroved after move.
    println!("{}", s); 
}

fn take_ownershop(some_string: String){
    // Moves ownership from s to some_string
    println!("{}". some_string);
}
```

Borrowing example

```rust
fn main(){
    let s1 = String::from("Hello world!");
    let length = calculate_lenght(&s1);
    println!("The length of '{}' is {}", s1, length);
}

// This funciton takes reference to a string
// This called borrowing
// References are immutable parameter s can't be changed
fn calculate_lenght(s: &String) -> usize{
    // Return string length
    return s.len(); 
}
```
