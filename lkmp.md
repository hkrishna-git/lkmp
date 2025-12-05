# 🐧 Linux Kernel Mentorship Program – Bug Fixing Track

Welcome to the **Linux Kernel Mentorship Program (Bug Fixing Track)** !  
This program is designed to help new developers to gain hands-on experience with the Linux kernel by learning the fundamentals of debugging, patch submission, and community collaboration.

---

## 🌟 Introduction
This **LKMP** program is motivated by the key goals:
- **Lower the barrier to entry**
- **Build confidence and gain skills**
- **Contribute to the community**

I have found **LFX** to be a valuable platform, as it not only provides structured online trainings but also creates opportunities to actively participate in open source programs. 
Through its mentorship and learning resources, LFX helps new contributors build practical skills, connect with experienced maintainers and gain real exposure to collaborative development in the Linux ecosystem.

Also, I have completed few online trainings through **LFX** which are very helpful in gaining knowledge.

I have noticed the **Linux Kernel Mentorship Program** in **LFX** while ongoing online trainings and began the journey with the application process, preliminary screening activities submission.

Once the completion of screening activities, I was selected and got an opportunity for this program.

---

## 📖 Overview

The Linux Kernel Mentorship Program provides a structured pathway for contributors to:
- Understand the kernel development workflow
- Identify and fix bugs in the kernel
- Learn how to submit patches following kernel coding standards
- Collaborate with maintainers and the wider open-source community

This track focuses specifically on **bug fixing**, which is one of the most effective ways to get started with kernel development. 
Though this track is for bug fixing, but it is not limited to that, mentors are encouraged to submit the new drivers / new design approaches to the drivers.

---

## 🎯 Goals

- Introduce mentees to the Linux kernel development process
- Choose the subsystems based on interest & dive into those
- Build confidence kernel development process
- Teach best practices for debugging and testing
- Provide guidance on submitting high-quality patches
- Foster long-term contributors to the Linux kernel community

---

## 🛠️ Getting Started

### 📋 Prerequisites
- Basic knowledge of C programming
- Familiarity with Git and version control
- A Linux environment
- Willingness to learn and collaborate

### 📧 Email Client
Configure the email client in the environment as it is needed to reply the messages.
There are many email client options are available. Use plain-text while composing/replying.
```
https://www.kernel.org/doc/html/v4.10/process/email-clients.html
```
I have used "alpine" as my email client and referred the below page for the basic configuration of the alpine email client
```
https://opensource.com/article/21/5/alpine-linux-email
```

### 🧑‍💻 Setup Instructions
1. Clone the Linux kernel source (recommended main branch):
   ```bash
   git clone https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
   ```
2. Configure and build the kernel as per the configuration
	- can be for x86
	- can be cross compiled for the actual target (ex : arm)
3. Create a minimal initramfs / rootfs using buildroot, if booting the kernel is via QEMU. If planning to boot in actual target then the rootfs shall be built for the corresponding board

---

## 🐞 Bug Fixing Workflow

### 🕵️ Identify a bug
The below are few ways to find bugs, understand the bug & contribute to chosen sub-system (not limited)
- syzkaller
- kernel documentation warnings
- removal deprecated APIs in kernel
- removal of vulnerable APIs in kernel
- explore TODO/FIXME comments (but to be cautious as some of them might be outdated)
- kernel bugzilla
- kernel CI issues
- using static analysis tools (ex : smatch)
- kernel regressions

### 🔬 Analyze and reproduce the bug
Analyze the bug and the scenario it has occurred, reproduce the bug in your environment.

### 🩹 Fix the bug
- Fix the code & write possible documentation
- Follow the kernel coding style (<linux_repo>/Documentation/process/coding-style.rst)

### 🧰 Test
- Test in your enviroment
	- run on hardware if the issue is common for all hardwares
	- run kunit tests
	- run selftests
	- run syzbot repro in your environment

### 🗳️ Submit the patch
- Commit modified code with the signoff flag (make sure to give full name)
	- Make sure to follow the commit message as per the subsystem (as it can vary subsystem to subsystem)
	- Make sure to add the commit description about the necessity of code change
- Prepare a patch using the git format-patch commands
- Check the patch using script (./scripts/checkpatch.pl)
- Get the maintainers of the subsystem using the script (./scripts/get_maintainer.pl)
- Send the patch using the configured email client

---

## 🔍 Subsystem Areas of Interest
My interest was on sound, so I have chosen ALSA subsystem as area of work.

### 🔊 ALSA (Advanced Linux Sound Architecture)
The ALSA subsystem is the primary sound system in the Linux kernel, providing audio and MIDI functionality.

#### 🧱 ALSA Architecture
ALSA is structured into several layers. On embedded systems and SoCs, the ASoC (ALSA System on Chip) framework adds an essential layer for modular audio driver design.

- Kernel driver
	- Hardware-specific drivers for sound cards and audio devices.
	- Handles low-level communication with audio hardware (PCI, USB, I2S/TDM interfaces)
- ASoC framework
	- Provides a modular architecture for embedded audio, splitting drivers into:
	- Codec drivers: Control audio codecs (DAC/ADC, mixers, routing, registers via I2C/SPI).
	- Platform drivers: Implement PCM DMA, audio interfaces (I2S/TDM), and buffer handling.
	- Machine drivers: Describe board-level CPU DAI and codec DAI, set up routes and constraints
- ALSA core layer
	- Common APIs for audio and MIDI handling
	- Manages PCM streams, controls, mixers, sequencer, and timer interfaces
	- Ensures behavioral consistency across different drivers (including ASoC drivers)
- User-space library (alsa-lib)
	- C library exposing ALSA functionality to applications (snd_pcm_*)
	- Provides configuration, plugin architecture (e.g., dmix, rate), and device discovery
	- Interfaces with topology and advanced routing when supported
- Applications layer
	- End-user applications and sound servers (e.g., aplay, arecord, PulseAudio, PipeWire)
		```
		Applications / Sound servers
					|
				alsa-lib
					|
				ALSA Core 
		(PCM, Control, Mixer, Sequencer)
					|
				ASoC Framework
		(Machine | Platform | Codec)
					|
		SoC/Board Audio Hardware
		```
  	- This is just an very high level overview, there is a lot inside the ALSA.
- References
	- <linux_repo>/Documentation/sound/
	- https://www.alsa-project.org/wiki/Main_Page
	- https://bootlin.com/doc/training/audio/

---

## 🚀 Journey
The journey through the mentorship program began with exploring different ways of contributions to chosen kernel subsystem.

The Exploration involved:
- Studying subsystem documentation and open source documentation
- Understanding the role of ALSA and ASoC in audio handling
- Identifying common bug categories and entry points for contributions
- Understanding the syzkaller bugs
- Creation of the syzkaller environment

Initially almost most of the time I was trying, exploring and understanding the different approaches, setting up the environemnt, experimenting the things.

---

## 📌 Patch Submissions
### ✅ Accepted Patches
The below patches were accepted in the sound sub-system and are available in maintainer's branch.
All being well this means that it will be integrated into the linux-next tree and sent to Linus during the next merge window (or sooner if it is a bug fix).

#### **ASoC: codec: wm8400: replace printk() calls with dev_*() device aware logging**
Replace direct printk() calls with the appropriate dev_*() logging
APIs.Use dev_err, dev_warn, dev_info, or dev_dbg to reflect the correct
severity level. Pass the canonical struct device pointer so logs
include device context and become traceable to specific hardware
instances.Improve log clarity, make messages filterable by device
and align the driver with kernel logging conventions to aid
debugging and maintenance.
```
- https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/commit/?id=6ef8e042cdcaabe3e3c68592ba8bfbaee2fa10a3
```

#### **ASoC: soc-core: check ops & auto_selectable_formats in snd_soc_dai_get_fmt() to prevent dereference error**
Smatch reported an issue that "ops" could be null (see
line 174) where later "ops" is dereferenced to extract
the dai fmts, also auto_selectable_formats can also be
null.
Add a proper null check before accessing both the ptrs
to ensure a safe execution.
```
- https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/commit/?id=e73b743bfe8a6ff4e05b5657d3f7586a17ac3ba0
```

#### **ASoC: tas2781: Replace deprecated strcpy() with strscpy()**
strcpy() is deprecated,use strscpy() instead.
Link: https://github.com/KSPP/linux/issues/88
```
- https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/commit/?id=3b071bdd26849172101081573a18022af108fb21
```

#### **ASoC: SOF: sof-client-probes: Replace snprintf() with scnprintf()**
As per the C99 standard snprintf() returns the length of the data
that *would have been* written if there were enough space for it.
It's generally considered safer to use the scnprintf() variant.
```
- https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/commit/?id=e45979641a9a9dbb48fc77c5b36a5091a92e7227
```

#### **ASoC: Intel: avs: Replace snprintf() with scnprintf()**
snprintf() as defined by the C99 standard,returns the
number of characters that *would have been* written if
enough space were available.Use scnprintf() that returns
the actual number of characters written.
```
- https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/commit/?id=df919994d323c7c86e32fa2745730136d58ada12
```

#### **ALSA: ac97: Fix kernel-doc warning for snd_ac97_reset**
kernel-doc populated the below warning for the non
static function "snd_ac97_reset".
"Warning: ./sound/ac97_bus.c:56 No description found
for return value of 'snd_ac97_reset'".
Added the return values as per the kernel-doc format.
```
- https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/commit/?id=ab3bd3662ed4605b7859988f224e2ed615f03060
```

#### **ALSA: rawmidi: Fix inconsistent indenting warning reported by smatch**
Fix smatch reported inconsistent indenting warning in rawmidi.
sound/core/rawmidi.c:2115 alsa_rawmidi_init() warn: inconsistent
indenting.
```
- https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/commit/?id=ef5e0a02d842b2c6dfcfd9b80feb185769b892ef
```

### 📤 Patches Under Review
The below patches were submitted for the review and waiting for the review.

#### **ALSA: usb-audio: Initialize status1 to fix uninitialized symbol errors**
Initialize 'status1' with a default value to resolve the static analysis
smatch reported error "uninitialized symbol 'status1'".
The 'status1' variable is used to create a buff using "kmemdup".
So, ensure to initialize the value before it is read.
```
- https://lore.kernel.org/all/20251204052201.16286-3-hariconscious@gmail.com/
```

#### **ASoC: codec: rt274: Use devm_request_threaded_irq to manage IRQ lifetime and fix smatch warning**
This patch replaces the manual management of IRQ with the device
managed IRQ API. Also, it removes the smatch reported warning.
sound/soc/codecs/rt274.c:1204 rt274_i2c_probe() warn: 
'rt274->i2c->irq' from request_threaded_irq() not released on lines: 1204.

Removed the "remove" function as IRQ is managed automatically.
Runtime testing is requested to see no regressions.
Please report any failures or unexpected behaviour, I will support in
updating the patch accordingly.

Similar warning is present in the codecs rt286 & rt298.
```
- https://lore.kernel.org/all/20251121140940.40678-4-hariconscious@gmail.com/
```

#### **ASoC: fsl: fsl_ssi: Replace deprecated strcpy() with strscpy()**
strcpy() is deprecated,use strscpy() instead.
```
- https://lore.kernel.org/all/29c40b5a-3e4d-e89d-ca22-a1059cca3480@gmail.com/
```

#### **sound/soc/sof:Use kmalloc_array instead of kmalloc**
Documentation/process/deprecated.rst recommends to avoid the use of
kmalloc with dynamic size calculations due to the risk of them
overflowing. This could lead to values wrapping around and a
smaller allocation being made than the caller was expecting.

Replace kmalloc() with kmalloc_array()
```
- https://lore.kernel.org/all/20250923142513.11005-1-hariconscious@gmail.com/
```

### 🔧 Patches under work in progress
The below patches are under work in progress where maintainer suggestions are incorporating.
#### **net/core : fix KMSAN: uninit value in tipc_rcv**
Syzbot reported an uninit-value bug on at kmalloc_reserve()

Syzbot KMSAN reported use of uninitialized memory originating from functions
"kmalloc_reserve()", where memory allocated via "kmem_cache_alloc_node()" or
"kmalloc_node_track_caller()" was not explicitly initialized.
This can lead to undefined behavior when the allocated buffer
is later accessed.

Fix this by requesting the initialized memory using the gfp flag
appended with the option "__GFP_ZERO".

https://syzkaller.appspot.com/bug?extid=9a4fbb77c9d4aacd3388
```
- https://lore.kernel.org/all/20250919180601.76152-1-hariconscious@gmail.com/
```

### ❌ Backfired patches
The below patches are backfired because of insufficient analysis, where underlying issues are misunderstood or overlooked during debugging. A thorough and methodical investigation is essential to ensure that patches address the real problem.
#### **sound/core/seq : fix data-race in snd_seq_fifo_cell_out/snd_seq_fifo_poll_wait**
data race in both the functions, snd_seq_fifo_cell_out &
snd_seq_fifo_poll_wait is protected with guards
https://syzkaller.appspot.com/bug?extid=c3dbc239259940ededba
```
- https://lore.kernel.org/all/20250916104547.27599-2-hariconscious@gmail.com/
```

#### **ALSA: timer: Fix null dereference of 'timer->card' reported by smatch**
Fix null dereference in snd_timer_proc_read().

Smatch reported that  was previously assumed to be null
at line 1226, but later accessed without a check.
This could lead to a null pointer dereference under certain conditions.

Add a null check before accessing to ensure safe execution.
```
- https://lore.kernel.org/all/20251103114902.11423-2-hariconscious@gmail.com/
```

---

## 🤝 Community & Mentorship
- Communication happens via mailing lists and messaging will be over IRC channels with the peers during initial screening phase of a program
- After the screening phase, will have dedicated channel to have a communication with the mentors / peers
- During office hours, mentees will have an opportunity to discuss over the issues. Also, mentors share the knowledge to move forward
- Also, mentors provide guidance, review patches, and help navigate the kernel community

---

## 🙌 Acknowledgements
Special thanks to the Mentors (**Shuah Khan & David Hunter**), Linux Foundation, kernel maintainers and the open-source community for supporting new contributors through mentorship and guidance.

