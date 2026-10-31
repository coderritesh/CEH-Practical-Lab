# SOP-02: Metasploitable 2 Vulnerable Machine Setup

## 1. Objective

The objective of this SOP is to configure Metasploitable 2 as the vulnerable target machine in the CEH Practical Lab using Oracle VirtualBox.

Metasploitable 2 is an intentionally vulnerable Linux virtual machine designed for cybersecurity training and authorized penetration-testing practice.

It will be used to understand:

- Network service enumeration
- Open ports and running services
- Vulnerable service configurations
- Linux security weaknesses
- Network security assessment
- Vulnerability assessment and penetration testing

## 2. Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox 7.x |
| Operating System | Metasploitable 2 |
| VM Name | Metasploitable 2 |
| RAM | 1 GB |
| CPU | 1 Core |
| Virtual Disk | Approximately 8 GB |
| Network Adapter | NAT Network |
| NAT Network Name | CEH-Lab-Network |
| IPv4 Address | 192.168.56.4/24 |
| Default Gateway | 192.168.56.1 |
| Username | msfadmin |
| Password | msfadmin |

**Note:** The IP address above is the address verified during this lab setup. DHCP can assign a different address later.

## 3. Requirements

Before starting, ensure the following are available:

1. Oracle VirtualBox 7.x installed.
2. Metasploitable 2 virtual machine archive.
3. At least 1 GB RAM available for the VM.
4. Sufficient free disk space.
5. NAT Network named `CEH-Lab-Network` already configured.
6. Kali Linux VM configured on the same NAT Network.

### Download

Metasploitable 2 is available from Rapid7:

https://information.rapid7.com/download-metasploitable.html

Download the Metasploitable 2 virtual machine archive from the official source.

## 4. Extract Metasploitable 2

### Step 1: Locate the downloaded archive

1. Open Windows File Explorer.
2. Navigate to the Downloads folder.
3. Locate the downloaded Metasploitable 2 archive.

### Step 2: Extract the archive

1. Right-click the downloaded archive.
2. Select **Extract All** or use 7-Zip.
3. Select the destination folder.
4. Click **Extract**.
5. Wait for extraction to finish.

### Step 3: Locate the virtual disk

1. Open the extracted folder.
2. Locate the Metasploitable 2 virtual machine files.
3. Find the `.vmdk` virtual disk file.

**Expected Result:** The extracted folder should contain the Metasploitable 2 virtual disk and its associated configuration files.

## 5. Create the Virtual Machine in VirtualBox

If the VM is already registered in VirtualBox, skip to Section 6.

### Step 1: Open VirtualBox

1. Open Oracle VirtualBox.
2. Click **New**.
3. Enter the following details:

| Field | Value |
|---|---|
| Name | Metasploitable 2 |
| Type | Linux |
| Version | Ubuntu (32-bit), if available |

4. Click **Next**.

### Step 2: Configure memory

1. Set Base Memory to `1024 MB`.
2. Click **Next**.

### Step 3: Configure the virtual disk

1. Select **Use an Existing Virtual Hard Disk File**.
2. Click the disk selection icon.
3. Click **Add**.
4. Browse to the extracted Metasploitable 2 folder.
5. Select the `.vmdk` file.
6. Click **Choose**.
7. Click **Next** or **Finish**, depending on the VirtualBox version.

**Expected Result:** The Metasploitable 2 VM should appear in the VirtualBox machine list.

## 6. Configure Virtual Machine Settings

### Step 1: Configure processor

1. Open VirtualBox.
2. Select Metasploitable 2.
3. Click **Settings**.
4. Select **System**.
5. Open the **Processor** tab.
6. Set Processor(s) to `1`.
7. Click **OK** after completing the remaining settings.

### Step 2: Verify memory

1. Select Metasploitable 2.
2. Click **Settings**.
3. Open **System → Motherboard**.
4. Verify Base Memory is `1024 MB`.

### Step 3: Verify virtual disk

1. Open **Settings**.
2. Select **Storage**.
3. Expand the storage controller.
4. Select the Metasploitable 2 `.vmdk` disk.
5. Check the disk information.
6. Verify that the expected virtual disk is attached.

**Note:** Do not create or attach another empty disk if the existing Metasploitable 2 disk is already attached.

### Step 4: Configure network adapter

1. Open Metasploitable 2 → **Settings**.
2. Select **Network**.
3. Open the **Adapter 1** tab.
4. Enable **Enable Network Adapter**.
5. Set Attached to: **NAT Network**.
6. Select Name: `CEH-Lab-Network`.
7. Ensure **Cable Connected** is enabled.
8. Click **OK**.

**Important:** Do not use Bridged Adapter for this intentionally vulnerable machine.

## 7. Start Metasploitable 2

1. Open Oracle VirtualBox.
2. Select Metasploitable 2.
3. Click **Start**.
4. Wait for the Linux operating system to boot.
5. Wait until the login prompt appears.

### Default login credentials

```text
Username: msfadmin
Password: msfadmin
```

Enter the username and password.

**Expected Result:** You should successfully log in and see the Metasploitable 2 terminal.

**Note:** The password will not be displayed while typing. This is normal Linux terminal behavior.

## 8. Verify Linux System Information

After logging in, execute the following commands.

### Step 1: Check operating system information

```bash
uname -a
```

This displays kernel and system information.

### Step 2: Check current username

```bash
whoami
```

Expected output:

```text
msfadmin
```

### Step 3: Check hostname

```bash
hostname
```

This displays the machine hostname.

### Step 4: Check disk space

```bash
df -h
```

This displays available and used filesystem space.

### Step 5: Check memory

```bash
free -m
```

This displays memory usage in megabytes.

## 9. Configure and Verify Network Connectivity

Metasploitable 2 must be connected to the same NAT Network as Kali Linux.

### Step 1: Check network interfaces

Execute:

```bash
ifconfig
```

Look for the network interface, usually `eth0`.

Expected lab configuration:

```text
eth0
inet addr:192.168.56.4
```

The actual IP address may differ because DHCP assigns addresses dynamically.

### Alternative command

If `ifconfig` is unavailable, execute:

```bash
ip addr
```

### Step 2: Check default gateway

Execute:

```bash
route -n
```

Look for the default route (`0.0.0.0`) pointing to:

```text
192.168.56.1
```

Alternatively, execute:

```bash
ip route
```

### Step 3: Test gateway connectivity

Execute:

```bash
ping -c 4 192.168.56.1
```

**Expected Result:** Successful replies from the gateway, provided ICMP is permitted.

### Step 4: Verify Kali Linux connectivity

Ensure Kali Linux is running on the same NAT Network.

On Kali Terminal, execute:

```bash
ip -br addr
```

Confirm Kali's current IP address.

Then execute:

```bash
ping -c 4 192.168.56.4
```

**Expected Result:** Kali Linux should receive replies from Metasploitable 2.

If DHCP assigns a different IP to Metasploitable 2, use its actual IP instead of `192.168.56.4`.

## 10. Verify Network Configuration from Kali Linux

On Kali Linux, verify the route and connectivity.

### Step 1: Check the route

```bash
ip route
```

Confirm the default gateway is correct.

### Step 2: Check target connectivity

```bash
ping -c 4 192.168.56.4
```

### Step 3: Check whether the target is reachable

```bash
ip neigh
```

This displays the neighbor table and may show the target's MAC address after communication.

**Expected Result:** The Kali and Metasploitable 2 machines should be able to communicate over the configured lab network.

## 11. Understand the Network Architecture

The current CEH lab uses a shared NAT Network.

```text
                 Host Machine
               Windows 10/11
                      |
              Oracle VirtualBox
                      |
              CEH-Lab-Network
                192.168.56.0/24
                      |
             Gateway: 192.168.56.1
                      |
          +-----------+-----------+
          |                       |
       Kali Linux            Metasploitable 2
       192.168.56.3           192.168.56.4
       Attacker                Vulnerable Target
```

### Explanation

- Kali Linux acts as the attacker machine.
- Metasploitable 2 acts as the vulnerable target.
- Both machines are connected to the same NAT Network.
- The gateway provides network connectivity.
- The machines can communicate with each other through the shared virtual network.

**Network Security Note:** NAT Network is not a completely isolated environment. It may provide outbound internet access. Keep vulnerable machines off Bridged Adapter and avoid unnecessary port forwarding. Use an Internal Network if complete network isolation is required.

## 12. Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| VM does not start | Incorrect VM configuration | Check RAM, processor, and virtual disk settings |
| Operating system does not boot | Incorrect disk attachment | Verify the correct `.vmdk` file is attached |
| Login fails | Incorrect credentials | Use the configured Metasploitable 2 credentials |
| No IP address | DHCP or adapter issue | Verify NAT Network and DHCP settings |
| Kali cannot ping target | Different virtual networks | Connect both VMs to `CEH-Lab-Network` |
| Gateway ping fails | Network configuration issue | Verify adapter status and routing |
| `ifconfig` command not found | Network tools unavailable | Try `ip addr` |
| VM becomes unresponsive | Host resource limitations | Shut down unnecessary VMs and check host resources |

## 13. Security Guidelines

1. Metasploitable 2 is intentionally vulnerable.
2. Use it only for authorized cybersecurity training.
3. Do not connect it to a production network.
4. Do not use Bridged Adapter.
5. Avoid unnecessary port forwarding.
6. Do not expose vulnerable services to the public internet.
7. Keep the VM on the intended lab network.
8. Use snapshots before major changes.
9. Do not store real credentials or sensitive information inside the vulnerable VM.

## 14. Final Verification Checklist

- [x] Metasploitable 2 virtual machine created/imported.
- [x] RAM configured to 1 GB.
- [x] CPU configured to 1 core.
- [x] Existing virtual disk attached.
- [x] NAT Network configured.
- [x] Login successful.
- [x] Linux system information verified.
- [x] Network interface verified.
- [x] IP address verified as `192.168.56.4`.
- [x] Default gateway verified as `192.168.56.1`.
- [x] Kali-to-Metasploitable connectivity verified.

## 15. Conclusion

Metasploitable 2 has been configured as the vulnerable target machine in the CEH Practical Lab.

The VM is connected to `CEH-Lab-Network` and can communicate with Kali Linux.

The environment is ready for authorized network enumeration, vulnerability assessment, and security-testing exercises.

---

**Document:** SOP-02  
**Project:** CEH Practical Lab  
**Platform:** Oracle VirtualBox  
**Operating System:** Metasploitable 2  
**Role:** Vulnerable Target Machine  
**Status:** Completed
