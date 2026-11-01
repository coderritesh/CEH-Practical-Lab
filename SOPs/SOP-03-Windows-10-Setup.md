# SOP-03: Windows 10 Client Setup

## 1. Objective

The objective of this SOP is to install and configure Windows 10 Pro in Oracle VirtualBox and connect it to the CEH Cybersecurity Lab Network.

After completing this SOP, the Windows 10 machine will be ready for:

- Windows administration practicals
- Network troubleshooting
- Active Directory domain joining
- Windows authentication testing
- Security monitoring
- Vulnerability assessment

## 2. Prerequisites

Before starting, make sure you have:

1. Oracle VirtualBox installed.
2. Windows 10 ISO file.
3. Sufficient free storage space.
4. An existing VirtualBox NAT Network named `CEH-Lab-Network`.
5. Kali Linux and Metasploitable 2 configured.

### Required Software

| Software | Purpose |
|---|---|
| Oracle VirtualBox | Virtual machine management |
| Windows 10 Pro ISO | Operating system installation |
| VirtualBox Guest Additions | Improved host and guest integration |

## 3. Lab Environment

### 3.1 Host Machine

| Component | Configuration |
|---|---|
| Host OS | Windows |
| Virtualization | Oracle VirtualBox |
| VM Storage Location | `E:\cyberlab\` |

### 3.2 Windows 10 VM Configuration

| Component | Configuration |
|---|---|
| VM Name | `Win10-Clientt` |
| Operating System | Windows 10 Pro |
| RAM | 4096 MB |
| CPU | 2 |
| Virtual Disk | 60 GB |
| Disk Format | VDI |
| Allocation | Dynamically Allocated |
| Firmware | BIOS |
| Network | NAT Network |

### 3.3 Storage Locations

| Item | Location |
|---|---|
| VM Folder | `E:\cyberlab\Win10-Clientt\` |
| Virtual Disk | `E:\cyberlab\Win10-Clientt\Win10-Clientt.vdi` |
| Windows ISO | `E:\cyberlab\win10-Client\Windows.iso` |

## 4. Prepare Windows 10 ISO

### Step 1: Create the ISO Folder

1. Open File Explorer on the host Windows machine.
2. Open the `E:\cyberlab\` directory.
3. Create a folder named `win10-Client`.
4. Place the Windows 10 ISO inside this folder.

Expected location:

```text
E:\cyberlab\win10-Client\Windows.iso
```

### Step 2: Verify the ISO

1. Open the folder.
2. Confirm that `Windows.iso` is present.
3. Check that the file size is reasonable for Windows installation media.
4. Make sure the file is accessible.

## 5. Create Windows 10 Virtual Machine

### Step 1: Open VirtualBox

1. Start Oracle VirtualBox.
2. Click **New**.
3. Enter the following details:

| Setting | Value |
|---|---|
| Name | `Win10-Clientt` |
| Type | Microsoft Windows |
| Version | Windows 10 (64-bit) |
| ISO Image | Select `Windows.iso` |

4. Continue to the hardware configuration.

### Step 2: Configure Hardware

Enter the following values:

| Setting | Value |
|---|---|
| Base Memory | 4096 MB |
| Processors | 2 |

Click **Next**.

### Step 3: Create Virtual Hard Disk

1. Select **Create a Virtual Hard Disk Now**.
2. Select VDI as the disk type.
3. Select **Dynamically Allocated**.
4. Set the disk size to 60 GB.
5. Set the VM storage location to:

```text
E:\cyberlab\Win10-Clientt\
```

6. Finish creating the virtual machine.

Expected result: VirtualBox Manager displays the new Windows 10 VM.

## 6. Configure VirtualBox Settings

Before starting the VM, configure its hardware and network.

### Step 1: Configure System Settings

1. Select `Win10-Clientt` in VirtualBox Manager.
2. Click **Settings**.
3. Open **System → Motherboard**.
4. Verify the following:

| Setting | Value |
|---|---|
| Base Memory | 4096 MB |
| Boot Order | Optical, Hard Disk |
| Firmware | BIOS |

5. Open **System → Processor**.
6. Set the processor count to 2.
7. Click **OK** after completing the configuration.

### Step 2: Configure Storage

1. Open **Settings → Storage**.
2. Select the SATA controller.
3. Verify the virtual disk and installation ISO.

| SATA Port | Device |
|---|---|
| Port 0 | `Win10-Clientt.vdi` |
| Port 1 | `Windows.iso` |

4. Confirm that the virtual disk is attached.
5. Confirm that the ISO is mounted.

### Step 3: Configure Network

1. Open **Settings → Network**.
2. Select **Adapter 1**.
3. Enable the network adapter.
4. Set **Attached To** to `NAT Network`.
5. Select `CEH-Lab-Network`.
6. Enable **Cable Connected**.
7. Click **OK**.

Expected result: The Windows VM is connected to the same virtual network as Kali Linux and Metasploitable 2.

## 7. Install Windows 10 Pro

### Step 1: Start the Virtual Machine

1. Select `Win10-Clientt`.
2. Click **Start**.
3. Wait for Windows Setup to load.

### Step 2: Select Installation Preferences

Select the following options:

| Setting | Value |
|---|---|
| Language | English |
| Time and Currency | Appropriate regional setting |
| Keyboard | US |

Click **Next**.

### Step 3: Start Installation

1. Click **Install Now**.
2. Enter a valid product key or select the available option to continue without entering one, where permitted.
3. Select **Windows 10 Pro**.
4. Accept the license terms.
5. Select **Custom: Install Windows only (advanced)**.

### Step 4: Select the Installation Disk

1. Select the unallocated space on the 60 GB virtual disk.
2. Click **Next**.
3. Allow Windows Setup to create the required partitions.
4. Wait for the installation to finish.

Windows may restart several times.

Expected result: Windows 10 first-time setup appears.

**Important:** Do not interrupt the installation or delete the virtual disk.

## 8. Complete Initial Windows Configuration

After installation, Windows displays the first-time setup screens.

### Step 1: Select Region and Keyboard

| Setting | Value |
|---|---|
| Region | India |
| Keyboard Layout | US |

Continue through the setup.

### Step 2: Create a Local User Account

1. Select the offline account or limited-experience option when offered.
2. Create a local account.

Use the following username:

```text
CEHUser
```

3. Set a dedicated lab password.
4. Complete the security questions.
5. Continue through the remaining setup screens.

Do not use personal or production passwords for the lab.

### Step 3: Complete Privacy Settings

1. Review the privacy settings.
2. Select the settings appropriate for your lab.
3. Complete the Windows setup.
4. Wait for the desktop to load.

Expected result: Windows 10 Pro desktop appears and the `CEHUser` account can log in.

## 9. Verify Windows Installation

### Step 1: Check Windows Version

1. Press `Windows + R`.
2. Enter:

```text
winver
```

3. Press Enter.

Expected installation details:

| Property | Value |
|---|---|
| Edition | Windows 10 Pro |
| Version | 22H2 |
| OS Build | 19045 |
| Architecture | 64-bit |

### Step 2: Check System Information

Open Command Prompt and execute:

```cmd
systeminfo
```

Check:

- OS Name
- OS Version
- System Type
- Total Physical Memory
- Computer Name

Expected result: Windows system information is displayed without errors.

## 10. Install VirtualBox Guest Additions

Guest Additions improves integration between VirtualBox and the Windows guest.

### Step 1: Insert Guest Additions CD

1. Start the Windows 10 VM.
2. Open the **Devices** menu in the VM window.
3. Select **Insert Guest Additions CD Image**.
4. Wait for Windows to detect the virtual CD.

### Step 2: Run the Installer

1. Open File Explorer.
2. Select **This PC**.
3. Open the VirtualBox Guest Additions CD drive.
4. Run:

```text
VBoxWindowsAdditions-amd64.exe
```

5. Approve the User Account Control prompt if displayed.
6. Follow the installation wizard.
7. Complete the installation.
8. Restart Windows when prompted.

### Step 3: Verify Installation

After restarting:

1. Check whether the mouse pointer moves smoothly between the host and guest.
2. Resize the VM window and verify display behavior.
3. Open Windows Apps or Programs and Features.
4. Confirm that VirtualBox Guest Additions is installed.

Expected result: Guest Additions is installed and mouse integration works.

## 11. Configure and Verify Network Connectivity

The Windows 10 client must communicate with the other lab machines.

### Step 1: Check IP Configuration

Open Command Prompt and execute:

```cmd
ipconfig /all
```

Find the active Ethernet adapter.

Expected configuration:

| Setting | Value |
|---|---|
| IPv4 Address | `192.168.56.6` |
| Subnet Mask | `255.255.255.0` |
| Default Gateway | `192.168.56.1` |

The address may change if the NAT Network DHCP configuration changes.

### Step 2: Test Gateway Connectivity

Execute:

```cmd
ping 192.168.56.1
```

Expected result: Replies from the gateway.

### Step 3: Test Kali Linux Connectivity

Execute:

```cmd
ping 192.168.56.x
```

Expected result: Replies from Kali Linux if ICMP traffic is allowed.

### Step 4: Test Metasploitable 2 Connectivity

Execute:

```cmd
ping 192.168.56.x
```

Expected result: Replies from Metasploitable 2 if ICMP traffic is allowed.

### Step 5: Verify the Network Configuration

The Windows client should be connected to:

```text
CEH-Lab-Network
```

Expected result: The Windows client can communicate with the other lab machines.

**Note:** A failed ping does not always mean the machine is unreachable. Firewall rules may block ICMP traffic.

## 12. Configure Windows Firewall

Windows Defender Firewall controls network traffic entering and leaving the system.

### Step 1: Open Firewall Settings

1. Open the Start menu.
2. Search for **Windows Security**.
3. Open **Firewall & network protection**.

### Step 2: Verify Firewall Status

1. Check the active network profile.
2. Confirm that Microsoft Defender Firewall is enabled.

Expected result: Firewall protection is enabled.

Do not permanently disable Windows Firewall for convenience. Configure only the specific rules required for a practical.

## 13. Verify Microsoft Defender

### Step 1: Open Windows Security

1. Open Start.
2. Search for **Windows Security**.
3. Open **Virus & threat protection**.

### Step 2: Check Protection Status

Verify:

- Real-time protection
- Cloud-delivered protection
- Security intelligence status

Expected result: Microsoft Defender protection is enabled and available.

## 14. Install Windows Updates

### Step 1: Open Windows Update

Navigate to:

```text
Settings → Update & Security → Windows Update
```

### Step 2: Check for Updates

1. Click **Check for updates**.
2. Wait for Windows to search.
3. Allow available updates to download and install.
4. If Windows requests a restart, restart the VM.
5. After restarting, open Windows Update again.
6. Check whether more updates or a restart are pending.

### Step 3: Verify Update Status

Confirm:

- Windows reaches the desktop.
- Update installation is no longer running.
- No restart is pending.
- Windows Update displays its current status.

Expected result: Windows Update completes successfully and Windows can boot normally.

## 15. Troubleshooting Windows Update

### Problem: Windows Stuck on "Getting Windows Ready"

During this lab setup, Windows displayed:

```text
Getting Windows ready. Don't turn off your computer.
```

The spinning dots stopped moving and the screen remained unchanged for more than one hour.

### Troubleshooting Procedure

1. Wait for Windows to finish processing the update.
2. Check whether the spinning dots are moving.
3. Check whether VirtualBox still shows the VM as Running.
4. If the guest remains unresponsive for an extended period, attempt a graceful shutdown:
   - Open the VM window.
   - Select **Machine → ACPI Shutdown**.
5. Wait for Windows to respond.

If ACPI Shutdown does not work and the VM remains completely frozen:

1. Close the VM window.
2. Select **Power Off the Machine**.
3. Confirm the operation.
4. Wait until VirtualBox shows the VM as Powered Off.
5. Start the existing VM again.
6. Check whether Windows boots successfully.
7. Verify Windows Update status.

**Important:** Forcing power off during an update is a last resort. It can interrupt the update and may cause Windows to roll back changes or require repair. Do not select Reset, Discard, or delete the VM as a recovery shortcut.

### Result in This Lab

After the frozen VM was powered off and started again, Windows successfully reached the desktop and the remaining configuration checks were completed.

## 16. Create a Baseline Snapshot

A baseline snapshot provides a recovery point for the Windows 10 client after setup and validation.

### Step 1: Shut Down Windows

1. Open the Windows Start menu.
2. Select **Power → Shut down**.
3. Wait until VirtualBox shows the VM as Powered Off.

### Step 2: Create the Snapshot

1. Open VirtualBox Manager.
2. Select `Win10-Clientt`.
3. Open **Snapshots**.
4. Click **Take**.
5. Enter the following name:

```text
Win10-Client-Baseline
```

6. Enter the following description:

```text
Windows 10 Pro installation completed.
Guest Additions installed.
CEH-Lab-Network configured.
Firewall and Microsoft Defender verified.
Windows Update completed.
Client connectivity validated.
```

7. Confirm snapshot creation.
8. Verify that the snapshot appears in the snapshot list.

**Important:** A snapshot is not a substitute for an independent backup. Keep separate backups of important VM files. Do not delete the baseline snapshot without understanding the recovery implications.

## 17. Final Verification Checklist

| No. | Task | Expected Result |
|---|---|---|
| 1 | Windows installation | Completed |
| 2 | Windows version | Windows 10 Pro 22H2 |
| 3 | OS build | 19045 |
| 4 | Local user account | `CEHUser` |
| 5 | Guest Additions | Installed |
| 6 | Network adapter | Connected |
| 7 | IP configuration | Verified |
| 8 | Gateway connectivity | Verified |
| 9 | Kali connectivity | Verified |
| 10 | Metasploitable connectivity | Verified |
| 11 | Windows Firewall | Enabled |
| 12 | Microsoft Defender | Enabled |
| 13 | Windows Update | Completed |
| 14 | Normal Windows boot | Verified |
| 15 | Baseline snapshot | Created |

## 18. Final Configuration Summary

| Component | Final Value |
|---|---|
| VM Name | `Win10-Clientt` |
| Operating System | Windows 10 Pro |
| Version | 22H2 |
| Build | 19045 |
| Architecture | 64-bit |
| RAM | 4096 MB |
| CPU | 2 |
| Virtual Disk | 60 GB |
| Network | `CEH-Lab-Network` |
| IPv4 Address | `192.168.56.6` |
| Gateway | `192.168.56.1` |
| Guest Additions | Installed |
| Firewall | Enabled |
| Microsoft Defender | Enabled |
| Windows Update | Completed |
| Snapshot | `Win10-Client-Baseline` |

## 19. Completion Status

**SOP-03: Windows 10 Client Setup — COMPLETED**

The Windows 10 Pro client VM was installed and configured in Oracle VirtualBox.

The network configuration, Guest Additions, Windows Firewall, Microsoft Defender, and Windows Update were verified. The client was tested for communication with the other lab machines, and a baseline snapshot was created.

## 20. Next SOP

**SOP-04: Windows Server 2022 Setup**

The next SOP will cover:

1. Windows Server 2022 ISO preparation.
2. Virtual machine creation.
3. Windows Server installation.
4. Initial server configuration.
5. Hostname configuration.
6. Static IP configuration.
7. Windows Update.
8. Server validation.

After the server setup, we will configure Active Directory Domain Services and DNS, then join the Windows 10 client to the domain.

## 21. Document Update History

| Version | Date | Description |
|---|---|---|
| 1.0 | 2026-11-01 | Windows 10 Client Setup and Configuration SOP created |

