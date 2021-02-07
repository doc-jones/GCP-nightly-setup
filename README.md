# GCP Rust nightly Setup
```yaml
Steps to setup, install and configure 
GCP Cloud Shell:  
Cloud Shell Editor:   
QEMU: 
Rust nightly:  
```
  
My journey with setting up an alternative dev environment for running Rust nightly began with the goal of running it on my Win10 machine. I needed to run nightly in support of building this project.  https://github.com/doc-jones/simple-os  
  
There are two issues with dependencies that I found with setting up nightly on Win10. First, it requires Microsoft Visual Studio C++ Tools which comes with GBs of data and sencond the Win10 scripts require use of the nightly-x86_64-pc-windows-msvc toolchain.  There is a known and well reported issue with using the msvc toolchain the the recommended fix is the use gcc instead with this command:  rustup install toolchain nightly-x86_64-pc-windows-gnc.  

# Usage  

## Step 1  

Setup an account using the free tier of the Google Cloud Platform. https://cloud.google.com/free/ . You will receive a $300 allotment in your GCP account, but the best news is that use of Cloud Shell doesn't incur any charges and is free forever.  

Once your account is setup, click the 1st icon in the list on the top right of the Google Cloud Platorm page as pictured below to **Activate the Cloud Shell**. It is a tiny terminal icon.  

![Activate Cloud Shell](https://user-images.githubusercontent.com/37349558/107147554-b3b64600-691c-11eb-8fd2-0b576af2da11.png)  

We will see a familiar terminal window that we can use to start installing Rust and other dependencies.  

![Cloud Shell View](https://user-images.githubusercontent.com/37349558/107151882-06026180-6933-11eb-99b2-640628512192.png)  

## Step 2

### Install Rust
  
Use rustup to install Rust  
`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`  

Next set Rust to nightly by running this command in your Cloud Shell/terminal window.  
`rustup toolchain install nightly`

And set to nightly run this commmand in your Cloud Shell.  
`rustup default nightly`  

### Install QEMU
  
Execute this command to install the Debian version of the QEMU elumator.  
`sudo apt install qemu qemu-system-x86 nasm -y`  
  
Installing QEMU in our Cloud Shell differs from the process for MacOS or Windows because we don't have the ability to launch a new window which is how QEMU would work on your local OS.
```yaml
DO NOT Try to cargo run or run QEMU at this point:
```  
  
There are changes that need to be made to your Cargo.toml file before you can can do cargo run.  
  
### Use an Executable Script Instead
  
In the root directory of your Cloud Shell, open a new file with any name you like. It must have the `.sh` extention.  My example as follows.  
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

