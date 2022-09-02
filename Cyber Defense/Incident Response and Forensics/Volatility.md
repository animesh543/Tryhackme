# Obtaining Memory Samples

Obtaining a memory capture from machines can be done in numerous ways, however,  the easiest method will often vary depending on what you're working  with. For example, live machines (turned on) can have their memory  captured with one of the following tools:

- FTK Imager - [Link](https://accessdata.com/product-download/ftk-imager-version-4-2-0)
- Redline - [Link](https://www.fireeye.com/services/freeware/redline.html) **Requires registration but Redline has a very nice GUI*
- DumpIt.exe
- win32dd.exe / win64dd.exe - **Has fantastic psexec support, great for IT departments if your EDR solution doesn't support this*

These tools will typically output a .raw file which contains an image of the  system memory. The .raw format is one of the most common memory file  types you will see in the wild.

Offline machines, however, can have their memory pulled relatively easily as  long as their drives aren't encrypted. For Windows systems, this can be  done via pulling the following file: 

**%SystemDrive%/hiberfil.sys**

hiberfil.sys, better known as the Windows hibernation file contains a compressed  memory image from the previous boot. Microsoft Windows systems use this  in order to provide faster boot-up times, however, we can use this file  in our case for some memory forensics!

Things get even more exciting when we start to talk about virtual machines and memory captures. Here's a quick sampling of the memory capture  process/file containing a memory image for different virtual machine  hypervisors:

- VMware - .vmem file
- Hyper-V - .bin file
- Parallels - .mem file
- VirtualBox - .sav file **This is only a partial memory file. You'll need to dump memory like a normal bare-metal system for this hypervisor*

These files can often be found simply in the data store of the corresponding  hypervisor and often can be simply copied without shutting the  associated virtual machine off. This allows for virtually zero  disturbance to the virtual machine, preserving it's forensic integrity.

# Examining Our Patient                            

1. First, let's figure out what profile  we need to use. Profiles determine how Volatility treats our memory  image since every version of Windows is a little bit different. Let's  see our options now with the command `volatility -f MEMORY_FILE.raw  imageinfo`
2. Running the imageinfo command in Volatility will provide us with a  number of profiles we can test with, however, only one will be correct.  We can test these profiles using the pslist command, validating our  profile selection by the sheer number of returned results. Do this now  with the command `volatility -f MEMORY_FILE.raw --profile=PROFILE  pslist`. What profile is correct for this memory image?
3. In addition to viewing active  processes, we can also view active network connections at the time of  image creation! Let's do this now with the command `volatility -f  MEMORY_FILE.raw --profile=PROFILE netscan`. Unfortunately, something not great is going to happen here due to the sheer age of the target  operating system as the command netscan doesn't support it.
4. It's fairly common for malware to attempt to hide itself and the process associated with it. That being said, we can view intentionally hidden  processes via the command `psxview`. What process has only one 'False'  listed?
5. Processes aren't the only area we're concerned with when we're examining a machine. Using the `apihooks` command we can view unexpected patches  in the standard system DLLs. If we see an instance where Hooking module: <unknown> that's really bad. This command will take a while to  run, however, it will show you all of the extraneous code introduced by  the malware
6. Injected code can be a huge issue and is highly indicative of very very bad things. We can check for this  with the command `malfind`. Using the full command `volatility -f  MEMORY_FILE.raw --profile=PROFILE malfind -D <Destination  Directory>` we can not only find this code, but also dump it to our  specified directory. Let's do this now! We'll use this dump later for  more analysis. How many files does this generate?
7.  These are commonly subjected to hijacking and other side-loading  attacks, making them a key target for forensics. Let's list all of the  DLLs in memory now with the command `dlllist`
8.  Now that we've seen all of the DLLs  running in memory, let's go a step further and pull them out! Do this  now with the command `volatility -f MEMORY_FILE.raw --profile=PROFILE  --pid=PID dlldump -D <Destination Directory>` where the PID is the process ID of the infected process we identified earlier (questions  five and six).

# Extra

**AlienVault Open Threat Exchange (OTX)** - An open-source threat tracking system. Create pulses based on your malware analysis work and check out the work of others. [Link](https://otx.alienvault.com/dashboard/new)

**SANS 408** - Windows Forensic Analysis [Link](https://www.sans.org/blog/for408-windows-forensic-analysis-has-been-renumbered-to-for500-windows-forensics-analysis/)

["Memory Forensics with Vol(a|u)tility"](https://youtu.be/dB5852eAgpc) - A great talk on learning the basics of Volatility and the GUI plugin [VolUtility](https://github.com/kevthehermit/VolUtility)

**MemLabs** - A collection of CTF-style memory forensic labs [Link](https://github.com/stuxnet999/MemLabs)

[Volatility 2.4. Windows & Linux Profile Cheatsheets](https://downloads.volatilityfoundation.org/releases/2.4/CheatSheet_v2.4.pdf)
