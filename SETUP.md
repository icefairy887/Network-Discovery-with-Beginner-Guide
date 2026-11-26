# Network-Discovery-for-Beginners
A network discovery and device fingerprinting lab using Windows, WSL2, Kali Linux and evidence-based analysis.


# Network Discovery — beginner setup

# What you need before you start

- A Windows 11 computer with administrator access and internet access for installation. The WSL commands also support sufficiently recent Windows 10, but the walkthrough assumes Windows 11.
- One phone or other device you own and can inspect.
- The computer and device on the same test network. Use only devices and a network you own or have permission to test.
- Time to restart Windows during setup.
Setup: Keep two terminal windows open: Windows PowerShell and Kali. The label above each code block tells you where to paste it. Copy only the command text, not the prompt, Run:, Output:, or the code-block


# What each thing does

Thing	       What it does here
WSL2	       Runs Linux inside Windows
Kali Linux	 The Linux environment where the scan commands run
Nmap	       Checks ports and compares service/OS fingerprints
Wireshark	   Windows packet-capture application
TShark	     Wireshark's command-line capture tool
Npcap	       The Windows capture driver Wireshark/TShark needs
arp-scan	   Looks for IPv4 neighbors on a directly attached local segment
OpenSSL	     Shows what happens during a TLS connection attempt
Python 3	   Runs the optional single-message protocol test


**Beginner note: Installing Wireshark on Windows normally offers Npcap during setup. Installing TShark inside Kali does not install Windows Npcap. You need both sides for this workflow.** 

# Setup 1 — install Kali through WSL

Right-click Start → Terminal (Admin), then choose PowerShell. **Run: wsl --list --verbose**

If kali-linux is already listed with VERSION 2, skip installation and open it with: **wsl -d kali-linux**

Otherwise, in administrator PowerShell: **wsl --list --online**
                                        **wsl --install -d kali-linux**

Restart Windows if asked. Then open Kali Linux from Start or run: **wsl -d kali-linux**

*On its first launch, create a Linux username and password. These do not have to match your Windows login. When you type the Linux password, nothing appears on screen. Type it anyway and press Enter. Later, sudo asks for this Linux password and you will also need it to login to your Kali VM.*

Back in Windows PowerShell, verify: **wsl --list --verbose**

The Kali entry should show VERSION 2. If an existing installation shows VERSION 1, save any open Kali work, then run: **wsl --set-version kali-linux 2**

# Troubleshooting Setup

## Errors 

“No installed distributions” means WSL may be present, but Kali still needs to be installed.

If the install asks for a restart, restart before continuing

If the download remains at 0%, Microsoft documents this alternative: **wsl --install --web-download -d kali-linux**

Virtual-machine/virtualization errors need the Windows WSL troubleshooting steps and possibly firmware virtualization enabled. Do not delete an existing distribution to fix an error; that can erase its files.

Do not disable antivirus or Windows security to get past an installation problem.

**Official links**

[Microsoft WSL Install](https://learn.microsoft.com/windows/wsl/install)

[Kali WSL setup](https://www.kali.org/docs/wsl/wsl-preparations/)


# Setup 2 — install the tools inside Kali

Open Kali, then run these one at a time: **sudo apt update**
                                         **sudo apt install -y nmap tshark arp-scan openssl python3**
*If TShark asks whether non-superusers should capture packets, you can choose No for this walkthrough; its Linux capture commands use sudo.*

Wait until the prompt returns. Then check: **nmap --version**
                                           **tshark --version**
                                           **arp-scan --version**
                                           **openssl version**
                                           **python3 --version**
                                           
Expected: **each command reports a version instead of “command not found.” Version numbers do not need to match the original investigation.**

*Beginner note: You do not need a Kali desktop, Win-KeX, VNC or Metasploit to follow this tutorial. The Kali terminal is enough.*


# Setup 3 — install Wireshark and Npcap on Windows

1. In Windows, open [the official Wireshark download page](https://www.wireshark.org/download.html).

2. Choose the Windows installer for your computer's architecture; *most Intel/AMD PCs use x64.*

3. Run the installer. Include the TShark component.

4. Include Npcap when the installer offers it. Keep ordinary defaults unless your administrator has different requirements.

5. Complete installation and restart if asked.

If Wireshark is already installed, first check whether TShark exists and can list interfaces. If Npcap is missing, get it through the official Wireshark installer or [Npcap's official site.](https://npcap.com/#download) Avoid old WinPcap downloads.

This tutorial uses normal Wi-Fi/Ethernet capture; monitor mode and raw 802.11 capture are not required. Installing a driver may require administrator approval on the test computer.

Official reference: [Wireshark installation on Windows.](https://www.wireshark.org/docs/wsug_html_chunked/ChBuildInstallWinInstall.html)


# Setup 4 — check that Windows can see its capture interfaces

In Windows PowerShell: **& "C:\Program Files\Wireshark\tshark.exe" -D**

*If you installed Wireshark somewhere else, use that actual location.*

Expected: **a numbered interface list. Find the active physical Wi-Fi or Ethernet adapter. Do not automatically choose WSL, VPN or loopback.**

To see Windows' adapter names: **Get-NetAdapter | Format-Table Name,InterfaceDescription,Status -AutoSize**
                               **Get-NetIPConfiguration**

Now check that Kali can call the Windows copy of TShark: **"/mnt/c/Program Files/Wireshark/tshark.exe" -D**

Expected: **the Windows capture-interface list again. This is the standard installed path used in this edition. The original notebook used a user-specific portable-app path; that path has been replaced.**

If Kali cannot launch a Windows executable, verify Windows interop is available. You can still run the capture from Windows PowerShell: **& "C:\Program Files\Wireshark\tshark.exe" -i 3 -n -a duration:10 -f "arp or udp port 5353"**

Replace 3 with your physical interface number. In Kali, the same test is: *"/mnt/c/Program Files/Wireshark/tshark.exe" -i 3 -n -a duration:10 -f "arp or udp port 5353"*

Expected: **it starts capturing and ends after ten seconds.** Zero packets can happen on a quiet network; it does not automatically mean setup failed. An access-denied error means the capture permissions/Npcap installation needs attention. If Npcap is restricted to administrators, use an elevated Windows terminal for the Windows capture.


*192.0.2.** and *2001:db8::* are documentation addresses. They are not live targets. **The MACs and dynamic ports above are synthetic replacements. Replace values in each command before running it.**

*For the simplest first test, use your own phone.*
On the phone, open the connected Wi-Fi network's details and privately note its IPv4 and current network MAC. This gives you a known target to learn with. 
Do not assume your phone will have an open high port, advertise ADB, or communicate with a Windows companion application. 

# Ready checkpoint:

- Kali opens and the five tool-version checks work.
- Windows TShark lists a physical adapter.
- A ten-second capture starts without a permission error.
- You know which terminal to use.
- You have your own test device and replacement values.

# Beginner 1st Run Checklist

## Start with one known device

Use your own phone and computer first. The point of the first run is to learn how each observation connects. You can repeat it as a blind identification exercise later.

Do the setup before running commands. Work through these checkpoints in order. If a checkpoint fails, record the exact error and stop that branch rather than guessing.

| Step | What to do | What counts as success |
| --- | --- | --- |
| 1 | Open Kali and run the five version checks | Each tool reports a version |
| 2 | List Windows capture interfaces | You can identify the physical active adapter |
| 3 | Run the ten-second capture smoke test | Capture starts and ends without a driver/permission error |
| 4 | Open the phone's current network details | You can privately note its current IP/MAC |
| 5 | Query Windows neighbors | You understand which rows belong to the physical interface; an absent phone is not an automatic failure |
| 6 | Run a 90-second discovery capture | You can distinguish your target traffic from unrelated broadcasts |
| 7 | Scan the one authorized device | Nmap finishes; report what actually responds |
| 8 | If a TCP port is open, try service/TLS probes | Record a recognized protocol or keep it unknown |
| 9 | If open/closed ports are available, try OS detection | Record guesses and warnings, including no exact match |
| 10 | If the PC/device exchange UDP, look up the Windows-side port | A contemporaneous process association, or an explained missing socket |
| 11 | If the process is CrossDeviceService, optionally inspect minimized metadata | Supporting OS/client-type context; skip if the cache does not exist |
| 12 | Compare current phone IP/MAC with network evidence | Matching current values, or a documented mismatch |
| 13 | Check IPv6 too | You understand that an IPv4-only filter is incomplete |
| 14 | Write separate device and service conclusions | An unknown service is an acceptable result |


# Branches you can skip

- No open TCP ports: skip the TLS, cipher and open/closed-port OS examples. Continue with captures, current settings and identity comparison.
- No CrossDevice/companion traffic: skip the process/cache branch. An unpaired phone will not necessarily create the original UDP conversation.
- No ADB advertisement or reason to test ADB: skip the optional greeting.
- No IPv6 displayed: record that observation; do not invent an IPv6 address.
- No traffic in a short capture: check the interface, wake your owned device and try a new bounded capture. The network may simply be quiet.


# *“Where do I paste this?”*

| What you see | Where it belongs |
| --- | --- |
| `wsl` commands | Windows PowerShell |
| `sudo`, `apt`, `nmap`, `openssl`, `python3` | Kali |
| `powershell.exe -NoProfile -Command ...` | The original notebook calls this from Kali |
| A standalone `Get-Net...` command | Windows PowerShell |
| `/mnt/c/Program Files/Wireshark/tshark.exe` | Kali calling a Windows executable |
| `C:\Program Files\Wireshark\tshark.exe` with `&` | Windows PowerShell |

*Beginner note: $ is sometimes the visible Linux prompt, and PS ...> is a Windows prompt. Neither is something you copy from an output example. A password prompt may show no characters as you type. Ctrl+C stops a live capture. Wait for scans to finish before pasting the next command.*

# If something doesn't work

| Problem | Try this |
| --- | --- |
| `sudo` or `apt` not recognized | You are probably in PowerShell; open Kali |
| `Get-NetNeighbor` not found in Kali | Use the original `powershell.exe` wrapper, or run `Get-NetNeighbor` in Windows PowerShell |
| `nmap`/`tshark`/`arp-scan` command not found | Complete Kali package installation and rerun its version check |
| Windows `tshark.exe` path missing | Verify TShark was included and check the actual Windows installation path |
| Capture denied/no useful interfaces | Check Npcap and capture permissions; use elevated Windows PowerShell if your Npcap configuration requires it |
| `arp-scan` finds only a virtual gateway | In NAT-mode WSL this is expected; use Windows' physical capture/neighbor view |
| A scan says Host is up but no ports answer | With `-Pn` this label can be an assumption; look for actual responses |
| Phone is missing from the neighbor cache | Wake it, inspect current network settings and observe new traffic; a cache is not a full inventory |
| Metadata file missing | Skip that optional branch; its schema/path is not guaranteed |
| Port changes between tests | Record the device state/time and resample carefully; do not assume a cause |
| TLS/ADB probe returns nothing | Report inconclusive; do not force an identity |

# Write down what happened

Keep exact IPs, MACs and timestamps in private notes. Use labels such as Computer A and Phone A in feedback you plan to share.

Test machine:
Windows version:
WSL/Kali versions:
Nmap/TShark/OpenSSL/Python versions:
Physical adapter selected:
Setup checkpoint reached:
Notebook section reached:
Command attempted, with identifiers removed:
Exact error, with identifiers removed:
What I expected:
What actually happened:
What wording confused me:
Device identity conclusion:
Remote service conclusion:
Steps skipped and why:

A completed run can correctly end with “I identified my device, but I still don't know what that TCP service is.” The tutorial should make the reasoning reproducible without requiring the original device or port behavior.
