---
title: "Challenge: Photon Lockdown"
date: 2024-05-17
tags: Challenge Hardware
---

# Photon Lockdown
**Challenge Author(s):** 41fr3d0

**Description:** We've located the adversary's location and must now secure access to their Optical Network Terminal to disable their internet connection. Fortunately, we've obtained a copy of the device's firmware, which is suspected to contain hardcoded credentials. Can you extract the password from it?

**Difficulty:** `easy`

**Flag:** `HTB{N0w_Y0u_C4n_L0g1n}`

[Challenge](https://github.com/Iarrova/FaRySYMD/raw/my-pages/_challenges/Photon%20Lockdown.zip)

## Writeup
In Photon Lockdown we are given the root file system (rootfs) of an Optical Network Terminal.

1. The first step consists in accessing the files in the file system. To do this, we just mount the `rootfs` disk into our system.
2. We then proceed to take a look at the files in the image. When we inspect the file system, we see the standard Linux directory structure.
3. We know that `/etc/` is a folder that acts as a central location for all the configuration files of the system, and inside this folder we find the `config_default.xml` file.
4. When we inspect this file, we see the standard configuration for the ONT, as well as an interesting entry with the name of `"SUPER PASSWORD"` which holds our flag as the value
