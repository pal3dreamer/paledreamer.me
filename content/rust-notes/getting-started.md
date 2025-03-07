# Run rust files using rustc
```
$ rustc main.rs
$ ./main
Hello, world!
```

# Using Cargo
## Creating a new project with Cargo
```
$ cargo new hello_cargo
$ cd hello_cargo
```
Dependencies can be added in `Cargo.toml` file
```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

## Building and running a cargo project
```
$ cargo build
```
Creates a file in the deubg folder 
If all goes well, Hello, world! should print to the terminal. Running cargo build for the first time also causes Cargo to create a new file at the top level: Cargo.lock
```
$ cargo run
```
Cargo run builds the executable and runs it. all in one command
```
$ cargo check
```
Can be used to see if your code compiles or not

