# BeagleBone Y-AI SDK install 
The steps to install the SDKs for BeagleBone Y-AI are explained below. According to TI's website, the linux SDK must be installed on Ubuntu-22. There are multiple platforms that you can install Ubuntu-22: Native hardware, Docker, WSL. Here I am going to go for WSL.

## Installing Ubuntu-22 on WSL
 Download Ubuntu-22 image from officail Ubuntu website using this [link](https://releases.ubuntu.com/jammy/). Download the file with .wsl extension and after downloading, install it by double clicking on it. 

## Installing SDKs 
There are 2 SDKs for AM67, the processor used in BeagleBone Y-AI. Linux SDK, RTOS SDK and bunch of toolchains necessary for compilation.

### Installing Linux SDK 
The linux SDK can be downloaded from this [link](https://www.ti.com/tool/PROCESSOR-SDK-AM67#downloads), provided by TI. After downloading the .bin file, copy it into Ubuntu-22 and then run it using commands below. Here I am using release "11_00_10_01". Notice you may download a newer release.

```
chmod +x ./ti-processor-sdk-linux-am67-sk-11_00_10_01-Linux-x86-Install.bin
sudo ./ti-processor-sdk-linux-am67-sk-11_00_10_01-Linux-x86-Install.bin
``` 
After running commands above, the SDK wizard asks you a path to install the contents, I suggest specifying a path that is safe from unintentional changes, for instance, I selected the path /opt/Y-AI/Linux, but you must create this directory using command below. 

```
sudo mkdir -p /opt/Y-AI/Linux
``` 

After a while, the linux SDK is installed. Now you should run the setup.sh script to install dependencies and configure other items. This script is interactive and asks you confirmations. I recommend checking out this [link](https://software-dl.ti.com/jacinto7/esd/processor-sdk-linux-am67/11_00_10_01/exports/docs/linux/Overview/Run_Setup_Scripts.html) before running it.

```
cd /opt/Y-AI/Linux
sudo ./setup.sh
```

### Installing RTOS SDK 
Unfortunately, TI has not public RTOS SDK for AM67, but we can use Jacinto processor family (like TDA4VM processor used in BeagleBone AI64) RTOS SDK, you can download the latest SDK release visiting this [link](https://www.ti.com/tool/PROCESSOR-SDK-J722S#downloads). 

Like before, I prefer to install SDK in directory beside the linux SDK. Thus, I copy the .tar.gz file into /opt/Y-AI (the directory created before) and then extract the contents. Ultimately, I will rename the extracted directory to "RTOS". The flow is like below, notice that I am using version 11_02_01_03, yours will be probably newer.

```
cd /opt/Y-AI
sudo tar -xf ti-processor-sdk-rtos-j722s-evm-11_02_01_03.tar.gz
sudo mv ti-processor-sdk-rtos-j722s-evm-11_02_01_03 RTOS
``` 

Now we have to setup the SDK using the "setup_psdk_rtos.sh", located at "RTOS/sdk_builder/scripts/setup_psdk_rtos.sh". But there are some issues with download links in the script, so I modified the "setup_psdk_rtos.sh" and did put it in this repo, replace your "setup_psdk_rtos.sh" with the one in the repo. This script will install the dependencies and downloads toolcahins for R5f and C7 dsp. For more detail about this script, you can refer to this [link](https://software-dl.ti.com/jacinto7/esd/processor-sdk-rtos-j722s/11_02_01_03/exports/docs/vision_apps/docs/user_guide/ENVIRONMENT_SETUP.html). Also there are another 2 files, which are misplaced in RTOS SDK directory, this probably will be fixed in future releases. Anyway, you must copy the "tisdk-adas-image-j722s-evm.tar.xz" and "boot-adas-j722s-evm.tar.gz" into "sdk_builder/scripts/".

```
cd /opt/Y-AI/RTOS/
mv tisdk-adas-image-j722s-evm.tar.xz sdk_builder/scripts/
mv boot-adas-j722s-evm.tar.gz sdk_builder/scripts/
cd sdk_builder/scripts/
# do not forget to replace your "setup_psdk_rtos.sh" with the one in this repo
./setup_psdk_rtos.sh
```