# kernel

### Hypervisor

The KVM-based hypervisor, its role is like qemu-system.

### Kernel

A extremely simple Linux kernel, supports few syscalls.

### User

Simple ELF(s) for testing our kernel.
Pre-built user program was provided, and you can re-generate by the following commands:
```sh
$ pip2 install pwntools
$ cd user
$ ./gen.py
```
NOTE: You have to install Python 2.x in advance.

## Setup

### Check KVM support

Check if your CPU supports virtualization:
```
$ egrep '(vmx|svm)' /proc/cpuinfo
```
NOTE: CPUs in VM might not support virtualization (i.e. no nested virtualization).
For example, EC2 on AWS doesn't support using KVM.

### Install KVM device

Check if KVM device exists:
```
$ ls -la /dev/kvm
```

If `/dev/kvm` is not found, you can enable it (on Ubuntu) with:
```
$ sudo apt install qemu-kvm
```

If you are not root, you need to add yourself into the `kvm` group to have permission for accessing `/dev/kvm`.
```
$ sudo usermod -a -G kvm `whoami`
```
Remember to logout and login to have the group changing effective.


## Development

```sh
$ make
$ hypervisor/hypervisor.elf kernel/kernel.bin user/orw.elf /etc/os-release
# NAME="Ubuntu"
# VERSION="18.04.1 LTS (Bionic Beaver)"
# ID=ubuntu
# ID_LIKE=debian
# PRETTY_NAME="Ubuntu 18.04.1 LTS"
# VERSION_ID="18.04"
# HOME_URL="https://www.ubuntu.com/"
# SUPPORT_URL="https://help.ubuntu.com/"
# BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
# PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
# VERSION_CODENAME=bionic
# UBUNTU_CODENAME=bionic
# +++ exited with 0 +++
```

### Environment

I only tested the code on Ubuntu 18.04, but I expected it works on all KVM-supported x86 Linux distributions. File an issue if you find it's not true.
