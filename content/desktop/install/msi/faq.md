---
title: MSI FAQs
description: Frequently asked questions for deploying Docker Desktop with Microsoft installer
keywords: msi, deploy, docker desktop, faqs
tags: [FAQ]
---

### What happens to user data if they have an older Docker Desktop installation (i.e. `.exe`)?

If they have an older `.exe` installation, users must [uninstall](../../uninstall.md) this version before using the new MSI version. This deletes all Docker containers, images, volumes, and other Docker-related data local to the machine, and removes the files generated by the application. For older versions, users should [backup](../../backup-and-restore.md) any containers that they want to keep.

For Docker Desktop versions 4.30 and later of the `exe` installer, a `-keep-data` flag is available. It removes Docker Desktop but keeps underlying data, such as the VMs that run containers.

```powershell
& 'C:\Program Files\Docker\Docker\Docker Desktop Installer.exe' uninstall -keep-data
```

### What happens if the user's machine has an older `.exe` installation?

The new MSI installer checks if a previous version was installed and doesn't proceed with the installation. Instead, it prompts the user to uninstall their current/old version first, before retrying to install the MSI version.

### My installation failed, how do I find out what happened?

MSI installations can sometimes fail unexpectedly and not provide users with much information about what went wrong.

To debug a failed installation, run the install again with verbose logging enabled:

```powershell
msiexec /i "DockerDesktop.msi" /L*V ".\msi.log"
```

After the installation has failed, open the log file and search for occurrences of `value 3`. This is the exit code Windows Installer outputs when it has failed. Just above the line, you will find the reason for the failure.

### Why does the installer prompt for a reboot at the end of every fresh installation?

The installer prompts for a reboot because it assumes that changes have been made to the system that require a reboot to finish their configuration.

For example, if you select the WSL engine, the installer adds the required Windows features. After these features are installed, the system reboots to complete configurations so the WSL engine is functional.

You can suppress reboots by using the `/norestart` option when launching the installer from the command line:

```powershell
msiexec /i "DockerDesktop.msi" /L*V ".\msi.log" /norestart
```
