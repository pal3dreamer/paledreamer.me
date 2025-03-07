# Basic Concepts
- In rust variables are immutable by default
- When a variable is immutable, once a value is bound to a name, you can’t change that value.
```
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         -
  |         |
  |         first assignment to `x`
  |         help: consider making this binding mutable: `mut x`
3 |     println!("The value of x is: {x}");
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable

For more information about this error, try `rustc --explain E0384`.
error: could not compile `variables` (bin "variables") due to 1 previous error
```
- To make variable mutable, use
```
let mut var = 6;
```
## Constants
- Like immutable variables, constants are values that are bound to a name and are not allowed to change, but there are a few differences between constants and variables.
- you aren’t allowed to use mut with constants. Constants aren’t just immutable by default—they’re always immutable
- The last difference is that constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.
- Note that there is a differnece between **compile time** and **runtime**. So compiler compiles the expression 
```
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```
at compile time, meanwhile runtime means when the programming is running of sorts. 

## Shadowing
```
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}
```
what it basically means is declaring a variable with the same name as a previous variable.

>Also something to note
>Most executable formats have special sections where you can store data that the code can reference >in one way or another.
>`static` explicitly puts the data in those special sections.
>`const` on the other hand just inlines the result of the expression where the constant is used.

# Data Types

