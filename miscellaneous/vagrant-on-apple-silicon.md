---
description: Vagrant on Mac M series chips
---

# Vagrant on Apple Silicon

Virtualbox is dead on Apple Silicon so my old Vagrant tricks don't work. Trying to get it to work with Parallels. [https://parallels.github.io/vagrant-parallels/docs/getting-started.html](https://parallels.github.io/vagrant-parallels/docs/getting-started.html)

First, mount the ISO from this path to install Parallels tools:

```
/Applications/Parallels Desktop.app/Contents/Resources/Tools/prl-tools-mac-arm.iso
```

Follow the installation steps.

Then run these commands inside the directory where you want to store the Vagrant config.

```sh
vagrant plugin install vagrant-parallels
vagrant init bento/ubuntu-20.04-arm64
```

TODO: Need to restart my machine and see how that went.
