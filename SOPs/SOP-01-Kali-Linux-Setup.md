# SOP-01: Kali Linux Attacker Machine Setup

## 1. Objective

The objective of this SOP is to configure Kali Linux as the attacker machine in the CEH Practical Lab using Oracle VirtualBox.

Kali Linux will be used for authorized cybersecurity learning and testing, including:

- Network scanning
- Vulnerability assessment
- Web application security testing
- Password security testing
- Network analysis
- Penetration testing in the authorized lab environment

## 2. Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox 7.x |
| Operating System | Kali Linux |
| Installation Method | Pre-built VirtualBox image |
| VM Name | Kali Linux |
| RAM | 4 GB |
| CPU | 2 Cores |
| Virtual Disk | Approximately 30 GB |
| Network Adapter | NAT Network |
| NAT Network Name | CEH-Lab-Network |
| IPv4 Address | 192.168.56.3/24 |
| Default Gateway | 192.168.56.1 |

**Note:** The IP address above is the address verified during this lab setup. DHCP can assign a different address later.

## 3. Requirements

Before starting, ensure the following are available:

1. Oracle VirtualBox 7.x installed on the host machine.
2. Kali Linux pre-built VirtualBox image.
3. At least 4 GB RAM available for the Kali VM.
4. At least 30 GB of available disk space for the virtual disk.
5. NAT Network named `CEH-Lab-Network` already configured in VirtualBox.

### Official Download Links

- Kali Linux: https://www.kali.org/get-kali/
- Official VirtualBox images documentation: https://www.kali.org/docs/virtualization/import-premade-virtualbox/
- Oracle VirtualBox: https://www.virtualbox.org/

Always download Kali images from the official Kali Linux website.

## 4. Download and Extract Kali Linux

### Step 1: Download the pre-built VirtualBox image

1. Open a web browser.
2. Visit https://www.kali.org/get-kali/
3. Find the Virtual Machines section.
4. Select the VirtualBox image.
5. Download the latest available image compatible with your VirtualBox version.
6. Wait for the download to complete.

### Step 2: Extract the downloaded archive

1. Open File Explorer.
2. Navigate to the Downloads folder.
3. Locate the downloaded Kali VirtualBox archive.
4. Right-click the archive.
5. Select **Extract All** or use 7-Zip → Extract Here.
6. Wait for extraction to complete.
7. Open the extracted folder.
8. Locate the `.vbox` configuration file and associated virtual disk file.

**Expected Result:** The extracted folder should contain the Kali VirtualBox configuration file and its virtual disk.

## 5. Import Kali Linux into VirtualBox

### Step 1: Add the existing virtual machine

1. Open Oracle VirtualBox.
2. From the top menu, click **Machine**.
3. Select **Add**.
4. Navigate to the folder where you extracted Kali Linux.
5. Select the Kali `.vbox` file.
6. Click **Open**.

**Expected Result:** Kali Linux should appear in the VirtualBox virtual machine list.

### Step 2: Verify VM configuration

1. Select Kali Linux in the VirtualBox left panel.
2. Click **Settings**.

#### A. Configure RAM

1. In Settings, click **System**.
2. Open the **Motherboard** tab.
3. Set Base Memory to **4096 MB**.
4. Click the **Processor** tab.
5. Set Processor(s) to **2 CPUs**.
6. Click **OK**.

#### B. Verify virtual disk

1. Select Kali Linux in the VirtualBox left panel.
2. Click **Settings**.
3. Select **Storage**.
4. Under the storage controller, select the Kali virtual disk (`.vdi`).
5. On the right side, check the **Virtual Size** value.
6. Verify that the disk is approximately 30 GB.

**Note:** If the existing disk is smaller or larger, do not resize it without checking the available disk space and the current VM configuration.

#### C. Configure network adapter

1. Open Kali Linux → **Settings**.
2. Select **Network**.
3. Open the **Adapter 1** tab.
4. Enable **Enable Network Adapter**.
5. Set Attached to: **NAT Network**.
6. Select Name: `CEH-Lab-Network`.
7. Ensure **Cable Connected** is enabled.
8. Click **OK**.

**Expected Result:** Kali Linux should be connected to the same NAT Network as the other lab machines.

## 6. Start Kali Linux

1. Select Kali Linux in VirtualBox.
2. Click **Start**.
3. Wait for the operating system to boot.
4. At the login screen, enter the credentials provided for the downloaded image.

For the official pre-built image, the commonly documented default credentials are:

- Username: `kali`
- Password: `kali`

If the downloaded image documentation specifies different credentials, follow that documentation.

**Expected Result:** Kali Linux desktop should load successfully.

## 7. Update Kali Linux

Keeping the operating system updated helps maintain package compatibility and security.

### Step 1: Open Terminal

1. Start Kali Linux.
2. Open the Terminal application from the desktop.

### Step 2: Update package lists

Execute:

```bash
sudo apt update
```

**Expected Result:** Kali should retrieve updated package information from its configured repositories.

### Step 3: Upgrade installed packages

Execute:

```bash
sudo apt full-upgrade -y
```

Wait until the process completes.

**Important:** This operation may take time and require additional disk space. Do not shut down the VM while the upgrade is running.

### Step 4: Restart Kali Linux

Execute:

```bash
sudo reboot
```

After the system restarts, log in again.

## 8. Install Essential Security Tools

Open Terminal and execute:

```bash
sudo apt install -y nmap gobuster nikto burpsuite metasploit-framework seclists wordlists curl wget net-tools
```

Wait for installation to complete.

### Verify installed tools

Run the following commands individually:

**Nmap**

```bash
nmap --version
```

**Gobuster**

```bash
gobuster version
```

**Nikto**

```bash
nikto -Version
```

**Metasploit Framework**

```bash
msfconsole --version
```

**Burp Suite**

```bash
burpsuite
```

**Expected Result:** The installed tools should display their version information or launch successfully.

Note: Some tools may have different command-line version options depending on the installed package version.

## 9. Configure and Verify RockYou Wordlist

RockYou is a commonly used password wordlist for authorized password-security exercises.

### Step 1: Check the wordlist directory

Execute:

```bash
ls -lh /usr/share/wordlists/
```

Look for `rockyou.txt.gz`.

### Step 2: Extract the wordlist

If `rockyou.txt.gz` exists, execute:

```bash
sudo gzip -dk /usr/share/wordlists/rockyou.txt.gz
```

The `-k` option keeps the original compressed file.

### Step 3: Verify the extracted file

Execute:

```bash
ls -lh /usr/share/wordlists/rockyou.txt
```

**Expected Result:** The extracted `rockyou.txt` file should be visible.

**Note:** Use wordlists only for password auditing and testing accounts within your authorized lab.

## 10. Verify Kali Network Configuration

The Kali VM must communicate with other machines in the CEH lab.

### Step 1: Check network interface

Execute:

```bash
ip -br addr
```

Expected lab interface information:

```text
eth0    UP    192.168.56.3/24
```

The interface name or IP may differ if DHCP assigns another address.

### Step 2: Check default gateway

Execute:

```bash
ip route
```

Expected route:

```text
default via 192.168.56.1 dev eth0
```

### Step 3: Test gateway connectivity

Execute:

```bash
ping -c 4 192.168.56.1
```

**Expected Result:** Successful replies from the gateway.

### Step 4: Test internet connectivity

Execute:

```bash
ping -c 4 8.8.8.8
```

**Expected Result:** Successful replies if outbound connectivity is available.

### Step 5: Test DNS resolution

Execute:

```bash
ping -c 4 google.com
```

**Expected Result:** The domain should resolve to an IP address and respond if DNS and outbound ICMP are available.

**Note:** A failed ping does not always mean the network is disconnected. Some networks block ICMP. Check the IP address, route, and DNS separately.

## 11. Verify Communication with Metasploitable 2

Metasploitable 2 is another VM in the CEH lab.

Ensure both VMs use:

`NAT Network → CEH-Lab-Network`

### Step 1: Start Metasploitable 2

1. Open VirtualBox.
2. Select Metasploitable 2.
3. Click **Start**.
4. Log in using the credentials configured for the VM.

### Step 2: Verify its IP address

On Metasploitable 2, execute:

```bash
ifconfig
```

The verified lab IP in this setup was:

```text
192.168.56.4
```

### Step 3: Test connectivity from Kali

On Kali Terminal, execute:

```bash
ping -c 4 192.168.56.4
```

**Expected Result:** Kali should receive replies from Metasploitable 2.

This confirms basic network communication between the two lab VMs.

## 12. Verify Metasploit Framework

Metasploit is used for authorized penetration-testing exercises.

### Step 1: Start Metasploit

Open Kali Terminal and execute:

```bash
msfconsole
```

Wait for the Metasploit console to load.

### Step 2: Verify the console

At the Metasploit prompt, execute:

```text
version
```

**Expected Result:** Metasploit should display its installed version.

To exit:

```text
exit
```

## 13. Create a Virtual Machine Snapshot

A snapshot provides a restore point before making major changes to the Kali VM.

### Steps

1. Shut down Kali Linux properly.
2. Open Oracle VirtualBox.
3. Select Kali Linux.
4. Open the **Snapshots** view.
5. Click **Take**.
6. Enter the snapshot name:

```text
SOP-01-Kali-Setup-Completed
```

7. Add a description:

```text
Kali Linux configured with required resources, CEH-Lab-Network connectivity, updated packages, and essential security tools.
```

8. Click **OK**.

**Expected Result:** The snapshot should appear in the Snapshots list.

## 14. Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| Kali VM does not start | Incorrect VM configuration | Check VirtualBox settings and available host RAM |
| No network connection | Incorrect network adapter | Select NAT Network and `CEH-Lab-Network` |
| No IP address | DHCP issue | Verify DHCP is enabled in VirtualBox NAT Network settings |
| Cannot ping gateway | Network configuration issue | Check interface status and default route |
| Cannot access internet | NAT or DNS issue | Check gateway connectivity and DNS resolution |
| Package update fails | Repository or internet issue | Verify connectivity and repository configuration |
| RockYou file not found | Wordlist package not installed | Install the `wordlists` package |
| Metasploit does not start | Installation or package issue | Verify package installation and inspect the error |

## 15. Security Guidelines

1. Use Kali only for authorized security testing.
2. Keep vulnerable lab machines on the intended lab network.
3. Do not use Bridged Adapter for vulnerable lab machines.
4. Do not expose vulnerable services to the public internet.
5. Avoid unnecessary port forwarding.
6. Take snapshots before making significant configuration changes.
7. Do not upload passwords, private keys, tokens, or sensitive configuration files to GitHub.
8. Remove private or sensitive information from screenshots before publishing them.

**Network note:** NAT Network allows lab VMs to communicate and may allow outbound internet access. It is not a fully isolated network. Use an Internal Network if a completely disconnected lab is required.




## 16. Final Verification Checklist

- [x] Kali Linux VM created/imported.
- [x] RAM configured to 4 GB.
- [x] CPU configured to 2 cores.
- [x] Virtual disk verified.
- [x] NAT Network configured.
- [x] Kali IP address verified as `192.168.56.3`.
- [x] Default gateway verified as `192.168.56.1`.
- [x] Gateway connectivity verified.
- [x] Internet connectivity verified.
- [x] DNS resolution verified.
- [x] Metasploitable 2 connectivity verified.
- [x] Essential security tools installed and verified.
- [x] Metasploit verified.
- [x] Snapshot created.

**Lab Status:** Kali Linux attacker machine configured and basic network connectivity verified.

## 17. Conclusion

Kali Linux has been configured as the attacker machine in the CEH Practical Lab.

The machine is connected to the `CEH-Lab-Network` NAT Network and can communicate with Metasploitable 2.

The environment is ready for the next stage of authorized cybersecurity lab setup and practical exercises.

---

**Document:** SOP-01  
**Project:** CEH Practical Lab  
**Platform:** Oracle VirtualBox  
**Operating System:** Kali Linux  
**Status:** Completed
