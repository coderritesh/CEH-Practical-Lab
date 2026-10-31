# SOP 00: MASTER LAB ARCHITECTURE & ORACLE VIRTUALBOX SETUP

**Document Code:** CEH-LAB-SOP-00  
**Target Audience:** Students & Self-Paced Learners  
**System Target:** Windows Host Machine with 16 GB Physical RAM  
**Standard Hypervisor:** Oracle VM VirtualBox 7.x  
**Lab Network:** CEH-Lab-Network

---

## 1. Executive Summary & Purpose

The purpose of this SOP is to establish a standardized cybersecurity lab environment using Oracle VM VirtualBox 7.x.

All lab virtual machines will use one shared VirtualBox NAT Network named `CEH-Lab-Network`.

**Standard Lab Network Configuration:**

- Network Name: `CEH-Lab-Network`
- IPv4 Prefix: `192.168.56.0/24`
- DHCP: Enabled
- Expected Gateway: `192.168.56.1`

This configuration allows the lab virtual machines to communicate with each other and, when permitted by the host network, access the internet through NAT.

**Important:** NAT Network does not mean complete isolation from the internet. Do not connect intentionally vulnerable machines using Bridged Adapter or expose their services through unnecessary port forwarding.

## 2. Master Lab Architecture & Resource Allocation

The following table defines the recommended hardware allocation for a Windows host with 16 GB physical RAM.

| Machine Role | Virtual Machine | vCPU | Assigned RAM | Virtual Disk | Network Mode | IP Assignment |
|---|---|---:|---:|---:|---|---|
| Attacker Station | Kali Linux | 2 | 4 GB | 30 GB | NAT Network | DHCP |
| Vulnerable Linux | Metasploitable 2 | 1 | 1 GB | 8 GB | NAT Network | DHCP |
| Vulnerable Client | Windows 10 Pro | 2 | 3.5 GB | 40 GB | NAT Network | DHCP |
| Domain Controller | Windows Server 2022 | 2 | 3.5 GB | 40 GB | NAT Network | DHCP |
| Host Operating System | Windows Physical PC | Host | 16 GB Physical RAM | — | Host | — |

**Total VM resource allocation:**

- Total vCPU allocation: 7
- Total assigned VM RAM: 12 GB
- Physical host RAM: 16 GB
- Remaining host RAM: Approximately 4 GB when all VMs are running
- Total nominal virtual disk capacity: Approximately 118 GB

**Important RAM Management Note:**

Leaving approximately 4 GB for Windows is a tight reserve. Running all four VMs simultaneously may cause system slowdown or freezing.

For better performance:

- Run only the VMs required for the current practical.
- Shut down unused VMs properly.
- Close unnecessary applications on Windows.
- Monitor RAM and CPU usage through Task Manager.

The virtual disk capacities shown above are maximum allocated capacities. Actual physical disk usage may be lower when dynamically allocated disks are used.

## 3. Step 1: Check BIOS Hardware Virtualization

Before creating or running virtual machines, confirm that hardware virtualization is enabled.

**Procedure:**

1. Start your Windows computer.
2. Press `Ctrl + Shift + Esc` to open Task Manager.
3. If Task Manager opens in compact mode, click **More details**.
4. Click the **Performance** tab.
5. From the left panel, select **CPU**.
6. Look at the CPU details displayed in the window.
7. Locate the **Virtualization** field.
8. Confirm that it displays:

`Virtualization: Enabled`

**If Virtualization is Disabled:**

1. Restart your computer.
2. During startup, press the BIOS/UEFI key specified by your computer manufacturer. Common keys include F2, Delete, Esc, or F10.
3. Open the CPU Configuration or Advanced Settings section.
4. Locate the virtualization setting.
5. Enable the appropriate option:
   - Intel Virtualization Technology (VT-x), or
   - AMD SVM / AMD-V.
6. Save the BIOS/UEFI changes.
7. Restart Windows.
8. Repeat the Task Manager verification.

**Expected Result:** Task Manager displays `Virtualization: Enabled`.

## 4. Step 2: Check or Install Oracle VirtualBox 7.x

If VirtualBox 7.x is already installed and working, verify its version and proceed to Step 3.

### 4.1 Check the Installed Version

1. Start your Windows computer.
2. Click the **Start** button.
3. Type `VirtualBox` in the search bar.
4. Open **Oracle VM VirtualBox**.
5. In VirtualBox Manager, click **Help** from the top menu.
6. Select **About VirtualBox**.
7. Check the version displayed in the About window.
8. Confirm that the installed version belongs to the VirtualBox 7.x series.
9. Close the About window.

### 4.2 Download VirtualBox (If Not Installed)

1. Open Chrome, Edge, or another web browser.
2. Visit the official Oracle VirtualBox download page:

https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html

3. Locate the current VirtualBox base package.
4. Find the **Windows hosts** option.
5. Click the Windows Installer download link.
6. Wait for the download to complete.
7. Open File Explorer.
8. Navigate to the Downloads folder.
9. Locate the downloaded VirtualBox installer.

### 4.3 Install VirtualBox

1. Right-click the downloaded installer.
2. Select **Run as administrator**.
3. If Windows displays a User Account Control prompt, click **Yes** after verifying the publisher.
4. In the installation wizard, click **Next**.
5. Review the installation location and features.
6. Keep the default selections unless you have a specific reason to change them.
7. Click **Next**.
8. If a network interface warning appears, save your work and close applications that depend on the network.
9. Click **Yes** or **Install** to continue, as prompted.
10. Wait for the installation to complete.
11. If Windows asks permission to install Oracle device software, verify the publisher and click **Install**.
12. Click **Finish**.
13. Restart Windows if requested.

After installation, open VirtualBox and verify the version using:

**Help → About VirtualBox**

## 5. Step 3: Install the VirtualBox Extension Pack (Optional)

The Extension Pack is not required for the basic NAT Network and Kali Linux setup in this lab.

Install it only if a specific lab requirement needs an Extension Pack feature.

**Important:** The Extension Pack version must exactly match the installed VirtualBox version.

**Procedure:**

1. Open the official Oracle VirtualBox download page:

https://www.oracle.com/virtualization/technologies/virtualbox-downloads.html

2. Locate the Extension Pack for your exact installed VirtualBox version.
3. Click the Extension Pack download link.
4. Wait for the `.vbox-extpack` file to download.
5. Open Oracle VM VirtualBox Manager.
6. Open the **Tools** area.
7. Locate and open **Extensions**.
8. Click **Install**.
9. Browse to the Downloads folder.
10. Select the downloaded `.vbox-extpack` file.
11. Click **Open**.
12. Review the license agreement.
13. If you accept the terms, scroll to the bottom and click **I Agree**.
14. Wait for installation to finish.
15. Confirm that Oracle VirtualBox Extension Pack appears in the Extensions list.

If the version does not match, stop and download the matching Extension Pack.

## 6. Step 4: Create the Isolated Lab NAT Network

All four lab virtual machines will use the same NAT Network.

### 6.1 Open Network Manager

1. Open **Oracle VM VirtualBox Manager**.
2. Click **File** in the top menu.
3. Move the mouse over **Tools**.
4. Click **Network Manager**.
5. In the Network Manager window, locate the **NAT Networks** tab.
6. Click the **NAT Networks** tab.

Depending on the VirtualBox 7.x layout, Network Manager may also be accessible from the Tools area.

### 6.2 Create a New NAT Network

1. In the NAT Networks tab, click the **Create** button, usually represented by a `+` icon.
2. A new NAT Network entry should appear.
3. Select the newly created network.
4. Locate the network configuration properties.
5. Enter the following values:

| Property | Value |
|---|---|
| Name | `CEH-Lab-Network` |
| IPv4 Prefix | `192.168.56.0/24` |
| Enable Network | Enabled, if shown |
| Enable DHCP | Checked |

6. Make sure the network name is entered exactly as:

`CEH-Lab-Network`

7. Make sure the IPv4 Prefix is entered exactly as:

`192.168.56.0/24`

8. Confirm that **Enable DHCP** is checked.
9. Click **Apply** to save the configuration.
10. Confirm that the network appears in the NAT Networks list.

### 6.3 Verify the NAT Network

Check that the displayed configuration contains:

- Name: `CEH-Lab-Network`
- IPv4 Prefix: `192.168.56.0/24`
- DHCP: Enabled
- Expected Gateway: `192.168.56.1`

**Expected Result:** The NAT Network is created successfully and is visible in VirtualBox Network Manager.

## 7. Step 5: Master Lab Network Architecture

The following diagram represents the planned lab architecture.

```text
          WINDOWS HOST COMPUTER
             16 GB PHYSICAL RAM
                     |
             ORACLE VIRTUALBOX 7.x
                     |
       NAT Network: CEH-Lab-Network
          Subnet: 192.168.56.0/24
             Gateway: 192.168.56.1
                DHCP: ENABLED
                     |
       +-------------+-------------+
       |             |             |
   KALI LINUX   METASPLOITABLE   WINDOWS 10
   DHCP IP       DHCP IP         DHCP IP
       |             |             |
       +-------------+-------------+
                     |
              WINDOWS SERVER 2022
                   DHCP IP
```

### Network Configuration for All VMs

For each virtual machine:

1. Open VirtualBox Manager.
2. Select the required VM from the left-side list.
3. Click **Settings**.
4. From the left-side menu, select **Network**.
5. Open the **Adapter 1** tab.
6. Check **Enable Network Adapter**.
7. From the **Attached to** drop-down menu, select **NAT Network**.
8. From the **Name** drop-down menu, select `CEH-Lab-Network`.
9. Check **Cable Connected**.
10. Click **OK** to save the settings.

Repeat these steps for Kali Linux, Metasploitable 2, Windows 10 Pro, and Windows Server 2022.

## 8. Step 6: IP Address Allocation and Verification

The lab uses DHCP for automatic IP address assignment.

**Important:** The IP addresses shown in older architecture diagrams or planning documents are examples only. DHCP does not guarantee that a specific VM will always receive the same IP address.

The actual IP addresses must be checked inside each virtual machine.

### 8.1 Find Kali Linux IP Address

1. Start the Kali Linux VM.
2. Sign in to Kali.
3. Open Terminal.
4. Execute:

```bash
ip -br addr
```

5. Identify the active network interface.
6. Record the assigned IPv4 address.

### 8.2 Find Metasploitable 2 IP Address

1. Start the Metasploitable 2 VM.
2. Sign in using the VM credentials.
3. Open the terminal.
4. Execute:

```bash
ifconfig
```

5. Locate the active network interface.
6. Record the assigned IPv4 address.

### 8.3 Find Windows 10 IP Address

1. Start the Windows 10 VM.
2. Sign in to Windows.
3. Press `Windows + R`.
4. Type `cmd`.
5. Press Enter.
6. Execute:

```cmd
ipconfig
```

7. Locate the active Ethernet adapter.
8. Record the IPv4 address.

### 8.4 Find Windows Server 2022 IP Address

1. Start the Windows Server 2022 VM.
2. Sign in to Windows Server.
3. Open Command Prompt.
4. Execute:

```cmd
ipconfig
```

5. Locate the active Ethernet adapter.
6. Record the IPv4 address.

**Important:** The expected gateway for this lab is `192.168.56.1`. Do not manually assign static IP addresses unless a later SOP explicitly instructs you to do so.

## 9. Step 7: Security Guidelines for the Lab

Follow these precautions throughout the lab:

1. Keep intentionally vulnerable machines connected to `CEH-Lab-Network`.
2. Do not use Bridged Adapter for vulnerable lab machines.
3. Do not configure unnecessary port forwarding.
4. Do not expose vulnerable services to the public internet.
5. Remember that NAT Network allows communication between lab VMs and may permit outbound internet access.
6. Use only systems and services for which you have explicit authorization.
7. Create snapshots before performing vulnerability assessments or making major configuration changes.
8. Shut down VMs that are not required for the current practical.

**Note:** NAT Network is not equivalent to a fully disconnected Internal Network. If a practical requires zero internet connectivity, use a separately planned isolated network configuration.

## 10. Step 8: Resource Management for a 16 GB Host

To avoid system slowdown, do not run all four virtual machines unnecessarily.

Suggested VM combinations:

| Practical | Recommended Running VMs |
|---|---|
| Kali to Metasploitable assessment | Kali + Metasploitable 2 |
| Windows client investigation | Kali + Windows 10 |
| Active Directory practical | Windows Server 2022 + Windows 10 |
| Network scanning practical | Kali + required target VM |

### Safely Shut Down a VM

1. Open the required VM window.
2. Save your work.
3. Use the guest operating system's normal shutdown option.
4. Wait until VirtualBox Manager displays the VM status as **Powered Off**.

Avoid using **Close → Power Off the Machine** unless the guest is unresponsive and normal shutdown is not possible.

## 11. Troubleshooting

| Problem | Recommended Action |
|---|---|
| Virtualization shows Disabled | Enable Intel VT-x or AMD-V/SVM in BIOS/UEFI and restart Windows. |
| VirtualBox installation fails | Run the official installer as administrator and review Windows security or driver prompts. |
| Extension Pack version mismatch | Download the Extension Pack matching the installed VirtualBox version. |
| NAT Networks tab not visible | Open File → Tools → Network Manager and check the available tabs. |
| Cannot create NAT Network | Reopen VirtualBox Manager with appropriate Windows permissions and retry. |
| VM does not receive an IP address | Check Adapter 1, NAT Network selection, Cable Connected, and DHCP settings. |
| VMs cannot communicate | Confirm that both VMs use the same `CEH-Lab-Network` and check their guest firewall settings. |
| VM performance is slow | Shut down unused VMs and check host RAM and CPU usage in Task Manager. |
| Internet is unavailable inside a VM | Check host internet connectivity and NAT Network configuration. |

## 12. Completion Checklist

Mark each item after verifying it.

- [ ] Windows host has 16 GB physical RAM.
- [ ] CPU hardware virtualization is enabled.
- [ ] Oracle VirtualBox 7.x is installed.
- [ ] Installed VirtualBox version is verified.
- [ ] Extension Pack installed only if required.
- [ ] Extension Pack version matches VirtualBox version, if installed.
- [ ] NAT Network `CEH-Lab-Network` created.
- [ ] IPv4 Prefix configured as `192.168.56.0/24`.
- [ ] DHCP enabled.
- [ ] Expected gateway `192.168.56.1` recorded.
- [ ] All four VMs planned to use the same NAT Network.
- [ ] IP address verification procedure understood.
- [ ] Security precautions reviewed.
- [ ] Host resource management plan reviewed.

## 13. Completion Criteria and Next Steps

SOP 00 can be marked as **COMPLETED** when:

1. Hardware virtualization is enabled.
2. Oracle VirtualBox 7.x is installed and verified.
3. The NAT Network `CEH-Lab-Network` is created.
4. The IPv4 prefix is `192.168.56.0/24`.
5. DHCP is enabled.
6. The expected gateway is recorded.
7. The network and resource configuration are ready for the individual VM setup procedures.

### Next SOPs

Proceed in the following order:

1. **SOP 01:** Kali Linux Attacker Machine Setup.
2. **SOP 02:** Metasploitable 2 Setup.
3. **SOP 03:** Windows 10 Vulnerable Client Setup.
4. **SOP 04:** Windows Server 2022 Domain Controller Setup.

---

**END OF SOP 00**

Document Code: CEH-LAB-SOP-00  
Document Title: Master Lab Architecture & Oracle VirtualBox Setup
