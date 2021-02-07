# GCP-nightly-setup
Steps to setup and configure GCP Cloud Shell and Cloud Shell Editor to use QEMU and Rust nightly.  
  
  
My journey with setting up an alternative dev environment for running Rust nightly began with the goal of running it on my Win10 machine. I needed to run nightly in support of building this project.  https://github.com/doc-jones/simple-os  
  
There are two issues with dependencies that I found with setting up nightly on Win10. First, it requires Microsoft C++ Visual Studio tools which comes with GBs of data and sencond the Win10 scripts require use of the nightly-x86_64-pc-windows-msvc toolchain.  There is a known and well reported issue with using the msvc toolchain the the recommended fix is the use gcc instead with this command:  rustup install toolchain nightly-x86_64-pc-windows-gnc.  

# Usage  

### Step 1  

Setup an account using the free tier of the Google Cloud Platform. https://cloud.google.com/free/  

Once your account is setup, click the 1st icon in the list on the top right of the Google Cloud Platorm page as pictured below to Activate the Cloud Shell. It is a tiny terminal icon.  

![Activate Cloud Shell](https://github.com/doc-jones/GCP-nightly-setup/issues/1#issue-802950597)



