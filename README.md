### Device specific configuration to build AOSP Android 13 for Raspberry Pi 4.

***

### How to build:
0. system: ubuntu-18.04

1. Establish [Android build environment](https://source.android.com/setup/initializing) and install [repo](https://source.android.com/docs/setup/develop#installing-repo).

2. Install additional packages:

```
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev 
gcc-multilib g++-multilib libc6-dev-i386 libncurses5 lib32ncurses5-dev x11proto-core-dev
libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig
bc coreutils dosfstools e2fsprogs fdisk kpartx mtools ninja-build pkg-config gcc-8 g++-8

sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 7
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 7
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 8
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 8

pip3 install meson==0.58
pip3 install ply mako

```

3. Initialize repo:

```
repo init -u https://android.googlesource.com/platform/manifest -b android-13.0.0_r24
curl --create-dirs -L -o .repo/local_manifests/manifest_brcm_rpi4.xml -O -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-13.0/manifest_brcm_rpi4.xml
```

You can also reduce download size by creating a shallow clone and removing unneeded projects (optional):

```
repo init -u https://android.googlesource.com/platform/manifest -b android-13.0.0_r24 --depth=1
curl --create-dirs -L -o .repo/local_manifests/remove_projects.xml -O -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-13.0/remove_projects.xml
curl --create-dirs -L -o .repo/local_manifests/manifest_brcm_rpi4.xml -O -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-13.0/manifest_brcm_rpi4.xml
```

4. Sync source code:

```
repo sync
```

5. Compile:

```
. build/envsetup.sh
lunch aosp_rpi4-userdebug
make bootimage systemimage vendorimage -j$(nproc)
```

6. Make flashable image:

```
./rpi4-mkimg.sh
```

Also look into [Linux kernel build instructions](https://github.com/raspberry-vanilla/android_kernel_manifest/tree/android-13.0).

***

### Issues:

- [Android](https://github.com/raspberry-vanilla/android_local_manifest/issues)
- [Linux kernel](https://github.com/raspberry-vanilla/android_kernel_manifest/issues)

***

### Wiki:

- [Audio](https://github.com/raspberry-vanilla/android_local_manifest/wiki/Audio)
- [DSI display](https://github.com/raspberry-vanilla/android_local_manifest/wiki/DSI-display)
- [HDMI display](https://github.com/raspberry-vanilla/android_local_manifest/wiki/HDMI-display)
- [Interfaces](https://github.com/raspberry-vanilla/android_local_manifest/wiki/Interfaces)
- [USB boot](https://github.com/raspberry-vanilla/android_local_manifest/wiki/USB-boot)
- [Video decoding](https://github.com/raspberry-vanilla/android_local_manifest/wiki/Video-decoding)
