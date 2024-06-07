# How to Download Rust Libraries with Cargo

Cargo is the Rust package manager, and it is used to manage Rust projects and their dependencies. This guide will walk you through the steps to download and use Rust libraries (also known as crates) with Cargo.

After installing, make sure Cargo is available by running:
```bash
cargo --version
```
### Create a new Rust Project

To create a new Rust Project , use the following command.
```bash
cargo new my_project
cd my_project
```
This creates a new directory named my_project with a simple Rust project template.

### Add Depenendencies 
Open the Cargo.toml file in your project directory. This file manages the dependencies for your project. To add a dependency, specify the crate name and version under the [dependencies] section.
For example, to add the rand crate (a popular library used to generate random numbers), add the following lines:
```toml
[dependencies]
rand = "1.0"
```
### Update and Build Your Project
After adding dependencies to Cargo.toml, run the following command to download and compile them:
```bash
cargo build
```
Cargo will fetch the specified crates from the crates.io registry and compile them along with your project.

### Use the Library in your Code 
Once the library is downloaded and compiled, you can use it in your Rust code. Open src/main.rs and include the crate at the top of the file:
```rust
use rand;
```

### Run your Project 
To run your project, use the following command:
```bash
cargo run
```
Cargo will build and execute your project, including any code that utilizes the libraries you added.
