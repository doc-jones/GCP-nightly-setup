# Google Cloud Platform Rust *nightly* Version Setup
```yaml
Steps to setup, install and configure  
GCP Cloud Shell:  
Rust nightly:  
QEMU:  
Cloud Shell Editor:  
```
  
My journey with setting up an alternative dev environment for running the *nightly* version of Rust began with the goal of running it on my Win10 machine. I needed to run *nightly* in support of building this project.  https://github.com/doc-jones/simple-os  
  
There are two issues with dependencies that I found with setting up *nightly* on Win10. First, it requires Microsoft Visual Studio C++ Tools which comes with GBs of data and sencond the Win10 scripts require use of the nightly-x86_64-pc-windows-msvc toolchain.  There is a known and well reported issue with using the msvc toolchain the the recommended fix is the use gcc instead with this command:  rustup install toolchain nightly-x86_64-pc-windows-gnc.  
  
The problem is that MS Visual Sudio continually switches back to the `msvc` toochain and that causes Rust *night* to fail to compile.

# Usage  

## Step 1  

Setup an account using the free tier of the Google Cloud Platform. https://cloud.google.com/free/ . You will receive a $300 allotment in your GCP account, but the best news is that use of Cloud Shell doesn't incur any charges and is free forever.  
  
Why? It's basically a free Linux development machine accessed through your web browser or mobile app.  
  
Once your account is setup, click the 1st icon in the list on the top right of the Google Cloud Platorm page as pictured below to **Activate the Cloud Shell**. It is a tiny terminal icon.  

![Activate Cloud Shell](https://user-images.githubusercontent.com/37349558/107147554-b3b64600-691c-11eb-8fd2-0b576af2da11.png)  

We will see a familiar terminal window that we can use to start installing Rust and other dependencies.  

![Cloud Shell View](https://user-images.githubusercontent.com/37349558/107151882-06026180-6933-11eb-99b2-640628512192.png)  

## Step 2

### Install Rust
  
Use rustup to install Rust  
`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`  
  
Update environment variable without the need to launch a new terminal window.   
`source $HOME/.cargo/env`

Next set Rust to *nightly* by running this command in your Cloud Shell/terminal window.  
`rustup toolchain install nightly`

And set to *nightly* run this commmand in your Cloud Shell.  
`rustup default nightly`  
   
Next add rust-src by running this command in the terminal.  
`rustup component add rust-src`  
  
Add llvm compiler tools execute this command at the terminal prompt.  
`rustup component add llvm-tools-preview`  

  
### Install QEMU
  
Execute this command to install the Debian version of the QEMU elumator.  
`sudo apt install qemu qemu-system-x86 nasm -y`  
  
Installing QEMU in our Cloud Shell differs from the process for MacOS or Windows because we don't have the ability to launch a new window which is how QEMU would work on your local OS.
```yaml
DO NOT Try to cargo run or run QEMU at this point:
```  
  
There are changes that need to be made to your `Cargo.toml` file before you can can do cargo run. We will need to make 2 changes to Cargo.toml of simple.os for QEMU to open in our Cloud Shell environment.  
  
  
### Use an Executable Script Instead
  
In the $HOME directory of your Cloud Shell, open a new file with any name you like. It must have the `.sh` extention.  My example as follows.  
`nano shellstarter.sh`  
  
This will open an empty file in the nano editor.  Copy/Paste the following code into your nano editor.  
```
sudo apt install qemu qemu-system-x86 nasm -y
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh &&
rustup toolchain install nightly
rustup default nightly
rustup component add rust-src
rustup component add llvm-tools-preview
```  
In nano `Ctrl + o` to write the file and `Ctrl + x` to exit the editor.   
Next at the terminal prompt make the script executable with `chmod +x shellstarter.sh` .  
Now you can run the file by typing `./shellstarter.sh` at a terminal prompt.  

The follow are the series of prompts you will see in your terminal window during the execution of this script.  I've added the correct response at the prompt in each of the screenshots below.  
    
  
```shell
$ ./shellstart.sh_
>
```

```shell
Current installation options:


     default host triple: x86_64-unknown-linux-gnu
       default toolchain: stable(default)
                 profile: default
    modify PATH variable: yes
    
1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
>2_
```

```shell
1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
>2

I am going to ask you the value of each of these installation options.
You may simply press the ENTER key to leave unchanged.

Default host triple?
_
```

```shell
I am going to ask you the value of each of these installation options.
You may simply press the ENTER key to leave unchanged.

Default host triple?


Default toolchain? (stable/beta/nightly/none)
nightly_
```

```shell
Default host triple?

Default toolchain? (stable/beta/nightly/none)
nightly

Profile (which tools and data to install)? (minimal/default/complete)
default_
```

```shell
Default toolchain? (stable/beta/nightly/none)
nightly

Profile (which tools and data to install)? (minimal/default/complete)
default

Modify PATH variable? (y/n)
y_
```

```shell
     default host triple: x86_64-unknown-linux-gnu
       default toolchain: stable(default)
                 profile: default
    modify PATH variable: yes
    
1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
1_
```

```shell
$ rustc --version
rustc 01.52.0-nightly (9778068cb 2021-02-07) 
```
  
## Step 3  
  
Cloud Shell comes with a number of languages and utilities already installed and one of those is `git`.  Follow your normal git workflow to Fork and clone https://github.com/doc-jones/simple-os   
  
Open the Cloud Shell Editor from the terminal window.  
  
![Open Editor](https://user-images.githubusercontent.com/37349558/107153253-7365c080-693a-11eb-8ae5-db2e124c2929.png)
  
Now let's pop the editor out to its own browser window.  
  
![Pop Out Shell](https://user-images.githubusercontent.com/37349558/107153363-1ae2f300-693b-11eb-852f-762a4a852eb6.png)
  
![Cloud Editor](https://user-images.githubusercontent.com/37349558/107153420-875df200-693b-11eb-99b9-a2a6c58e2107.png)
  
## Step 4
  
Open `Cargo.toml` in the editor and make these 2 changes.  
  
![Update Version](https://user-images.githubusercontent.com/37349558/107197609-1877ac00-69c2-11eb-9e91-e51219d37a7a.png)

![curses](https://user-images.githubusercontent.com/37349558/107197612-1877ac00-69c2-11eb-81c4-572b69e8c8c2.png)
  
The addition of curses allows QEMU to open in a terminal instead of needing to launch a new window.  Now you can use `cargo test` at the command line. If all is well, then you will see `Hello, World` displayed in the terminal.  
  
```yaml
cd into the simple-os directory then issue the next 3 commands at the terminal prompt.  
(ignore the colons they are a text style workaround)
cargo install bootimage :
cargo build          :
cargo run            :
```
  
![Qemu Hello World](https://user-images.githubusercontent.com/37349558/107153969-5206d380-693e-11eb-960a-90c2ad32ebff.png)

### Special Thanks
  
Thank you, Tom Tarpey https://github.com/decagondev for help with solving the sticky parts.
