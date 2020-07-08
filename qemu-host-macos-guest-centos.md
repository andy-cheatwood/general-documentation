# QEMU - macOS (host) - CentOS (guest)
## Overview
Documents how to install CentOS on QEMU on macOS.

***

1. Install Homebrew (if not already installed)
    - Run the following in Terminal:
    ```
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
    ```
    - More information: https://brew.sh/
2. Install QEMU (using brew)
    - Run the following in Terminal:
    ```
    brew install qemu
    ```
    - More information: https://www.qemu.org/download/#macos
3. Download CentOS
    - Find dvd/iso
    - Geographical mirrors: https://www.centos.org/download/mirrors/
    - More information: https://www.centos.org/download/
    - Direct link (one I used at the time): http://mirror.cs.vt.edu/pub/CentOS/8/isos/x86_64/CentOS-8.2.2004-x86_64-dvd1.iso
4. Create a disk image
    - Run the following in Terminal to create a qcow2 formatted disk image 10G in size.
    - _Note: path, image name and size can be whatever you'd like. The following is just a recommendation._
    ```
    sudo qemu-img create -f qcow2 /where/ever/you/want/to/NAME_WHATEVER.qcow2 10G
    ```
5. Launch QEMU with CentOS ISO attached
    - Run the following in Terminal:
    - _Note: modify the **cdrom** and **drive** paths to the location of the .iso and .qcow respectively._
    ```
    sudo qemu-system-x86_64 \
    -m 2048 \
    -vga virtio \
    -show-cursor \
    -usb \
    -device usb-tablet \
    -enable-kvm \
    -cdrom /full/path/to/downloaded/centos/iso/ISO_NAME.iso \
    -drive file=/full/path/to/disk/image/created/in/step/4/DISKIMAGE.qcow2,if=virtio \
    -machine accel=hvf \
    -cpu host
    ```
6. Install CentOS
    - Nothing special here, just follow prompts like normal.
    - Reboot when/if prompted
    - Finish configuration
    - Full shutdown
7. Run CentOS VM without cdrom/iso attached (aka: how you will normally run the VM)
    - Run the following in Terminal:
    ```
    sudo qemu-system-x86_64 \
    -m 2048 \
    -vga virtio \
    -show-cursor \
    -usb \
    -device usb-tablet \
    -enable-kvm \
    -drive file=/full/path/to/disk/image/created/in/step/4/DISKIMAGE.qcow2,if=virtio \
    -machine accel=hvf \
    -cpu host
    ```
    - Pro tip: create a script with the above code in it for easy running.
    - Create: /usr/local/bin/start-centos (or something)
    - Make it executable:
    ```
    sudo chmod +x /usr/local/bin/start-centos
    ```
    - Should be able to run **start-centos** from Terminal to launch the VM.
