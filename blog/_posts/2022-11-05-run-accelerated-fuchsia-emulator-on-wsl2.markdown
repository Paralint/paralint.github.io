---
author: ixe013
comments: false
date: 2022-11-05
layout: post
link: http://www.paralint.com/blog/2022/11/05/run-accelerated-fuchsia-emulator-on-wsl2
slug: find-new-modified-and-unversioned-subversion-files-on-windows
title: Run an hardware accelerated Fuchsia emulator on WSL2
categories:
- Other technical
- Fuchsia
---

[Fuchsia](https://fuchsia.dev/) is an open source and security oriented operating system developped
at Google. You can run it in an emulator on your Mac or Linux computer. But what about Windows? With WSL2
running on the hypervisor, all that is required is to enable nested virtualization and fix an access mask.

## KVM is not accelerated by default on WSL2

The [Getting started guide at fuchsia.dev](https://fuchsia.dev/fuchsia-src/get-started/sdk) is great, but I'll
use [the condensed version in the README file of the Fuchsia SDK](https://fuchsia.googlesource.com/sdk-samples/getting-started) for this
if you want to follow along.

If you copy and paste the commands in your terminal, they will all work except that you will get this message 
when attempting to run the emulator:

```
$ tools/ffx emu start workstation_eng.qemu-x64 --headless
Logging to "/home/ixe013/.local/share/Fuchsia/ffx/emu/instances/fuchsia-emulator/emulator.log"
Waiting for Fuchsia to start (up to 60 seconds).............................................................
After 60 seconds, the emulator has not responded to network queries.
The emulator process is still running (pid 506).
The emulator is configured to use user-mode/port-mapped network access.
Hardware acceleration is disabled, which significantly slows down the emulator.
You can execute `ffx target list` to keep monitoring the device, or `ffx emu stop` to terminate it.
You can also change the timeout if you keep encountering this message by executing `ffx config set emu.start.timeout <seconds>`.
```

Even if the emulator process is running, you will be able to connect to it. Notice this message:

> Hardware acceleration is disabled, which significantly slows down the emulator.

You see this for two reasons:
  1. Your account does not have permission on `/dev/kvm`
  1. Hardware acceleration is not enabled

The [Fuchsia instructions to enable VM acceleration on Linux](https://fuchsia.dev/fuchsia-src/get-started/set_up_femu#enable-vm-acceleration)
don't work in WSL2, we are going to fix that.


## Enable hardware accelerated emulation in WSL2

### Grant yourself rights to the KVM

First of all, make your user a member of the `kvm` group with this command. It is the same command that you would do in Linux.

```
sudo usermod -a -G kvm ${USER}
```

Unfortunately - for a reason I don't understand - `/dev/kvm` will always have `root:root` as its owner and primary group. 
To change that you must `chmod` at startup. Add the following section to the file `/etc/wsl.conf`:

```
[boot]
command = /bin/bash -c 'chown -v root:kvm /dev/kvm && chmod 660 /dev/kvm'
```

### Enable nested virtualization

Nested virtualization is not enabled by default in WSL2, at least not in version 21H2 (OS build 22000.1165). You don't need
to recompile your WSL distribution to enable nested virtualization, just add this section to your `/etc/wsl.conf`:

```
[wsl2]
nestedVirtualization=true
```

### Restart WSL2

You need to restart WSL2 for the changes to take effect. You can also restart your computer, but it is faster to close every terminal 
window and run the following command (using the Windows Key + R, or in an existing Powershell or CMD prompt):

```
wsl.exe --shutdown
``` 

## Trying it out

From that point on, you are done! Open a new terminal window to start WSL2 under its new configuration and just follow the Fuchsia emulator
startup instructions as if your were using Linux on bare metal.

```
$ tools/ffx emu start workstation_eng.qemu-x64 --headless
Logging to "/home/ixe013/.local/share/Fuchsia/ffx/emu/instances/fuchsia-emulator/emulator.log"
Waiting for Fuchsia to start (up to 60 seconds)...................................
Emulator is ready.
```

Enjoy!
