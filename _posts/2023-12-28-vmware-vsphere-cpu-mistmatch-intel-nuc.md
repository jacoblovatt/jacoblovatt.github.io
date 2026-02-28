---
layout: post
title: Intel NUC VMware ESXi - HW feature incompatibility - Fatal CPU Mismatch
subtitle: How to resolve the 'Fatal CPU mismatch' purple screen of death for VMware ESXi on Intel NUC
tags: [it, homelab, intel nuc, vmware, esxi, vsphere]
---

Reference: [virten.net][vitren-cpu-mismatch].

## Problem Description
When installing VMware ESXi 8.0 on my Intel NUC Core i5-1250P Wall Street Canyon, I was presented with the following purple screen of death at boot:

![VMware ESXi Fatal CPU Mismatch](/assets/img/blog/fatal-cpu-mismatch.png)

Key components of the error message:
```
HW feature incompatibility detected: cannot start  
...  
CPU_CheckUniformity@vmkernel#nover  
...  
Fatal CPU mismatch on feature "Hyperthreads per core"  
Fatal CPU mismatch on feature "Cores per package"  
Fatal CPU mismatch on feature ...  
```

## Cause
This problem is caused by the architecture of 12th-generation Intel CPUs, which have different core types: performance cores and efficiency cores.

The kernel parameter _cpuUniformityHardCheckPanic_ detects that the CPU architecture is not uniform and triggers a panic, which causes the purple screen.

## Resolution
You need to disable the _cpuUniformityHardCheckPanic_ behavior.

To do this manually:
1. You need to have a monitor and keyboard plugged in to your ESXi host.
2. As your ESXi host boots, press **Shift+O** to enter boot options.
3. Append **cpuUniformityHardCheckPanic=FALSE** as a boot option and press **Enter** to continue the boot sequence. This lets you bypass the check and boot your ESXi system.
4. To make this change permanent, once booted press **Alt+F1** to enter the CLI of your host (press **Alt+F2** to return to the main console at any time), enter your root credentials, and run:

```bash
esxcli system settings kernel set -s cpuUniformityHardCheckPanic -v FALSE
```

If **Alt+F1** is not working and you see a black screen, enable troubleshooting options. Press **F2**, enter your root credentials, go to _Troubleshooting Options_, and select _Enable ESXi Shell_.

5. Confirm the setting:

```bash
esxcli system settings kernel list -o "cpuUniformityHardCheckPanic"
```

You should see output similar to:

|Name|Type|Configured|Runtime|Default|Description|
|--- |--- |---       |---    |---    |---        |
|cpuUniformityHardCheckPanic|Bool|FALSE|FALSE|TRUE|Panic if CPU uniformity hard check fails|

Note: for more information on advanced ESXi kernel settings, see:
* [William Lam - ESXi Advanced Kernel Settings Reference](https://williamlam.com/2022/12/esxi-advanced-kernel-settings-reference.html)
* [VMware KB 1038578 - Configuring advanced options for ESXi/ESX](https://kb.vmware.com/s/article/1038578)

For more information about making this change in the ESXi installation ISO, and additional changes needed for 13th-gen Intel CPUs, see the post from [virten.com][vitren-cpu-mismatch].

[vitren-cpu-mismatch]: https://www.virten.net/2022/11/esxi-7-and-8-installation-fails-with-fatal-cpu-mismatch-on-feature/
