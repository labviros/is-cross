Cross compilation of linux kernel for the Raspberry Pi
==================================================================

## Build Configuration

Edit BRANCH, PATCH, KERNEL variables inside the **"compile"** script to configure your build. Then run the **"run"** script to start building the kernel (you only need **docker** installed for this). After the build setup is done a menu will pop up. Enable the settings you want to be built. To enable kernel preemption edit: "Kernel Features → Preemption Model → Fully Preemptible Kernel (RT)". After closing the configuration menu the build will proceed automatically.

## Installing the kernel

After building the kernel you can deploy it to your raspberry pi by running the **"run"** script inside the install directory.