# Ubuntu-Set-up-Nvidia-cuda-conda-pytorch
## a short guide to solving black screen after installing Nvidia-driver on Ubuntu

**Step 1.** install ubuntu 
1. download [ubuntu image](https://releases.ubuntu.com/focal/) to your flash device
2. download [rufus](https://rufus.ie/en/) to create the bootable USB


### Black screen 
Sometimes, you get a black screen and it's very annoying. To deal with the black screen:
1. press `ctrl` + `shift` + `ESC` + `F1` or **something else** when booting and try to get into the GNU-GRUB window. Note it's not the BIOS and you need to try several times to succeed.
2. select the **advanced option for ubuntu**
3. select the second term (recovery mode) and you are now in the recovery menu.
4. enter the root
5. change the grub setting with `sudo nano \etc\default\grub` and add 'nomodeset' before the `quite splash` and use `Ctrl` + `x` to save. The **nomodeset** will use the integrated gpu of the motherboard to boot your computer.
6. enter `exit` to exit from the root and you are now in the recovery menu.
7. enter the grub and it will update grub file that you modified in step 5
8. enter the resume to boot the computer

you should be good to boot your computer now.

**Step 2.** install the nvidia-driver
1. `sudo apt update` + `sudo apt upgrade` + `sudo apt install gcc`
2. `sudo apt install nvidia-driver-515 nvidia-dkms-515` Change the version number [515] of the driver that fits your gpu.
3. remove the `nomodeset` from the grub with  `sudo nano \etc\default\grub` and update your grub with `sudo update-grub`. Note that without step 3, you cannot reboot your computer.
4. reboot your computer and you can check whether the driver is installed with `nvidia-smi`

**Step 3.** install cuda. check [this video](https://www.youtube.com/watch?v=4gcqGxBIUnc&t=22s) before installing. You can install any version instead of the one shown in the video.
1. download the [cuda toolkit](https://developer.nvidia.com/cuda-toolkit-archive) that fits your GPU driver and OS
2. select runfile (local) as the Installer Type and it will generate the Installation Instructions. Note that it will be better to have `chmod +x` as suggested in the video. Just select install the **cuda** without the driver, the sample, and the document, as shown in the video.
3. `sudo nano ~/.bashrc` and add the path to your bashrc file. *Note change the 11.1 for your installed cuda version*
    ```shell
    export PATH=$PATH:/usr/local/cuda-11.1/bin 
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.1/lib64
    export CUDADIR=/usr/local/cuda-11.1
    ```
4. `source ~/.bashrc` and you should be good to get the cuda installed. You can check it with `nvcc -V` with a new terminal.

**Step 4.** install cudann **(to modify)**
1. download the [cudnn](https://developer.nvidia.com/cudnn). It needs you to login in and verify your email.
2. download the Deb file that fit your cuda and OS [not recommended!]. **Or** install it with the compressed file as show in the video above.
3. you can check whether cudnn is installed with `sudo apt search cudnn | grep installed` if you installed it with .deb. You can also check it
   with the [stackflow answers](https://stackoverflow.com/questions/31326015/how-to-verify-cudnn-installation).
   ```shell
   function lib_installed() { /sbin/ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH) 2>/dev/null | grep $1; }
   function check() { lib_installed $1 && echo "$1 is installed" || echo "ERROR: $1 is NOT installed"; }
   check libcudnn 
   ```

**Step 5.** install the conda
1. download conda from the [website](https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html) and install it.
2. if you'd prefer that conda's base environment not be activated on startup, you can use `conda config --set auto_activate_base false`.
3. create and activate the conda environmnet
   ```shell
   conda create -n first_env python=3.7
   conda activate first_env
   ```

**Step 6.** install pytorch
1. make sure your conda env is activated **recommended**
2. go to [pytorch website](https://pytorch.org/get-started/previous-versions/) and search the pytorch version that fits your cuda
3. install the pytorch with **pip**
4. you can check whether it got installed with
   ```shell
   python -c "import torch; print(torch.__version__)"
   python -c "import torch; print(torch.version.cuda)"
   ```
