---
layout: post
title: Intel NUC - HW feature incompatibility - Fatal CPU Mismatch
subtitle: How to resolve the 'Fatal CPU mismatch' purple screen of death for VMware vSphere ESXi on Intel NUC
tags: [it, homelab, intel nuc, vmware, esxi, vsphere]
---

Information taken from [virten.net][vitren-cpu-mismatch].

# Problem Description
When installing VMware ESXi 8.0 on my Intel NUC Core i5-1250P Wallstreet Canyon I am presented with the following purple screen of death

![VMware ESXi Fatal CPU Mismatch](/assets/img/blog/fatal-cpu-mismatch.png)

The key components of the error message
> HW feature incompatibility detected: cannot start  
> ...  
> CPU_CheckUniformity@vmkernel#nover  
> ...  
> Fatal CPU mismatch on feature "Hyperthreads per core"  
> Fatal CPU mismatch on feature "Cores per package"  
> Fatal CPU mismatch on feature ...  

# Cause
This problem is caused by the new architecture of Intel CPUs which are equipped with different types of cores - Performance-cores and Efficient-cores. 

The kernel parameter cpuUniformityHardCheckPanic complains that the CPU architecture is not uniform and "panics" which is the cause of the purple screen.

# Resolution
We need to disable the cpuUniformityHardCheckPanic behaviour.

To do this manually:
1. You need to have a monitor and keyboard plugged in to your ESXi host
2. As your ESXi host boots press **shift+o** to enter boot options
3. Append **cpuUniformityHardCheckPanic=FALSE** as a boot option and press **enter** to continue the boot sequence. This allows you to bypass the check behaviour and boot your ESXi system
4. To make this change permanent, once booted press **alt+f1** to enter the cli of your host (press **alt-+f2** to return to the main console at any time), enter root credentials to authentication, and run the following  
 Note: if **alt+f1** is not working for you and you are presenting with black screen, you need to enable troubleshooting options. Press **f2** and enter the root credentials, go to _Troubleshooting Options_, and _Enable ESXi Shell_
 > esxcli system settings kernel set -s cpuUniformityHardCheckPanic -v FALSE
 5. To confirm this setting  
 > esxcli system settings kernel list -o "cpuUniformityHardCheckPanic"

 You should see output similar to
 
 |Name|Type|Configured|Runtime|Default|Description|
 |--- |--- |---       |---    |---    |---        |
 |cpuUniformityHardCheckPanic|Bool|FALSE|FALSE|TRUE|Panic if CPU uniformity hard check fails|

 Note: for info on advanced esxi kernel settings see:
 * [William Lam - ESXi Advanced Kernel Settings Reference](https://williamlam.com/2022/12/esxi-advanced-kernel-settings-reference.html)
 * [VMware KB 1038578 - Configuring advanced options for ESXi/ESX](https://kb.vmware.com/s/article/1038578)

For more information about making this change as part of the ESXi installation material, and additional changes needed for 13th Gen Intel CPUs see the blog post from [virten.com][vitren-cpu-mismatch]

[vitren-cpu-mismatch]: https://www.virten.net/2022/11/esxi-7-and-8-installation-fails-with-fatal-cpu-mismatch-on-feature/
