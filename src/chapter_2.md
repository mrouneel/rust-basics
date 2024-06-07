# Installing Rust

Rust is a systems programming language that runs blazingly fast, prevents segfaults, and guarantees thread safety. Below are the instructions to download and install Rust on both Linux/Unix and Windows systems.

## Installation on Linux/Unix

### Install Dependencies

Before installing Rust, ensure you have the necessary dependencies.
In this guide I will show you the process of installing it in Debian-based Distros but it works similar in other Distributions.

For **Debian-based distributions** (like Ubuntu):
```bash
sudo apt update
sudo apt install build-essential curl
```
### Install Rust, rustup and Cargo

The official Rust Website recommends downloading Rust with rustup which is a tool for managing Rust versions. To install Rust with rustup, run the following command in your terminal:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
This command will not only download Rust and rustup, but also Cargo which is Rust's package manager.

### Verify the Installation

To verify that Rust is installed correctly, run:
```bash
rustc --version
```
## Installation on Windows

### Install the Rustup installer

I recommend downloading Rust on Windows from rustup.rs which provides an executable for Windows devices. [Rustup](https://rustup.rs/#)

### Update and Uninstall Rust

Rust is constantly being improved. To update to the latest version of Rust, use rustup:
```bash
rustup update
```
If you need to uninstall Rust for any reason, use rustup on the Unix Terminal or Windows Powershell/CMD:
```bash
rustup self uninstall
```
