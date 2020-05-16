---
layout: post
title: Bypass anti-debug and anti-vm of VMP
---

Today i'm gonna show you how to bypass anti-debug and anti-vm of VMProtect(version before 3.2)
# Overview

VMP has used a lot of anti debug and anti virtual machine techniques which makes it really hard to debug programs protected by it. actually, VMP has stored a **DWORD FLAG**, which indicates whether it should detect user mode debugger, kernel mode debugger, virtual machine detection etc. so here I will show you how to locate the **FLAG** variable in memory and change it to bypass all kinds of anti-debug and vm-detection techniques.

# Details

- Use windbg to load the VMP sample

```
g $exentry
```

After opened the VMP sample, use this command to reach the entry point of the sample.

- Break on kernel32!LocalAlloc

```
ba e1 kernel32!LocalAlloc; g;
```

Set a hardware breakpoint on the kernel32!LocalAlloc, then go.

- Search the **FLAG**

```
s @esp L1000 sample_image_default_load_base_address sample_image_current_load_base_address
```

Search the **FLAG** variable from the top of the stack, within a range of 0x1000 bytes, for example, default image base is 0x400000, and current image load base is 0xd60000, you should type as follows:

```
s @esp L1000 00 00 40 00 00 00 d6 00
```

- Change it to zero

The **FLAG** variable is stored just next to the current image load base. so if the search result of previous step is *0x12345678*, then type:

```
ed 0x12345678+8 0
```

# One picture shows everything

![_config.yml]({{ site.baseurl }} /images/bypass_vmp.png)
