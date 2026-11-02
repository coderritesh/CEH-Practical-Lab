# SOP-04: Windows Server 2022 Domain Controller Setup

## 1. Objective

The objective of this SOP is to install and configure Windows Server 2022 as the server machine in the CEH Practical Lab using Oracle VirtualBox.

Windows Server will act as the central server for the lab. It will be configured to provide:

- Active Directory Domain Services (AD DS)
- Domain authentication and centralized user management
- Domain Name System (DNS)
- Domain Controller services
- Centralized management for Windows client machines

The planned Active Directory domain is:

```text
ceh.lab
```

The Windows Server machine name is:

```text
CEH-DC01
```

## 2. Lab Environment

| Component                          | Configuration                           |
| ---------------------------------- | --------------------------------------- |
| Hypervisor                         | Oracle VirtualBox 7.2.22                |
| Operating System                   | Windows Server 2022 Standard Evaluation |
| VM Name                            | WinServer-DC01                          |
| Computer Name                      | CEH-DC01                                |
| RAM                                | 4 GB                                    |
| CPU                                | 2 Cores                                 |
| Virtual Disk                       | 50 GB, dynamically allocated            |
| Firmware                           | BIOS                                    |
| Network Adapter                    | NAT Network                             |
| NAT Network Name                   | CEH-Lab-Network                         |
| IPv4 Address                       | 192.168.56.7                            |
| Subnet Mask                        | 255.255.255.0                           |
| Default Gateway                    | 192.168.56.1                            |
| Current DNS before AD DS promotion | 192.168.0.1                             |
| Planned DNS after promotion        | 192.168.56.7                            |
| Planned Domain                     | ceh.lab                                 |
| NetBIOS Domain Name                | CEH                                     |

**Note:** The server IP address was assigned through a DHCP reservation in the VirtualBox NAT Network. The IP address should be verified if the network configuration is changed.

## 3. Requirements

Before starting, ensure the following are available:

1. Oracle VirtualBox 7.x installed on the host machine.
2. Windows Server 2022 ISO file.
3. At least 4 GB RAM available for the server VM.
4. At least 50 GB of available disk space.
5. Existing VirtualBox NAT Network named `CEH-Lab-Network`.
6. Working network connectivity between the lab machines.

### Official Download Links

- Microsoft Windows Server evaluation center: [https://www.microsoft.com/en-us/evalcenter/](https://www.microsoft.com/en-us/evalcenter/)
- Oracle VirtualBox: [https://www.virtualbox.org/](https://www.virtualbox.org/)

Always obtain Windows Server installation media from Microsoft's official website.

## 4. Windows Server Installation Media

The Windows Server ISO used in this lab:

```text
SERVER_EVAL_x64FRE_en-us.iso
```

ISO location:

```text
E:\cyberlab\winsever\SERVER_EVAL_x64FRE_en-us.iso
```

The ISO was already available before starting this setup.

## 5. Create the Windows Server Virtual Machine

### Step 1: Open VirtualBox

1. Open Oracle VirtualBox on the host Windows machine.
2. Click **New**.
3. Enter the virtual machine name:

```text
WinServer-DC01
```

4. Select the Windows Server 2022 ISO file.
5. Select the appropriate Microsoft Windows server operating system type and version if prompted.
6. Continue with the VM creation wizard.

### Step 2: Configure Hardware

Configure the following resources:

| Setting         | Value                 |
| --------------- | --------------------- |
| Base Memory     | 4096 MB               |
| Processors      | 2                     |
| Virtual Disk    | 50 GB                 |
| Disk Allocation | Dynamically allocated |

### Step 3: Configure Virtual Disk

1. Create a new virtual hard disk.
2. Select VDI format.
3. Select dynamically allocated storage.
4. Set the disk size to 50 GB.
5. Complete the VM creation process.

**Expected Result:** The `WinServer-DC01` virtual machine should appear in VirtualBox.

## 6. Configure the Virtual Network

The Windows Server VM must use the same NAT Network as the other CEH lab machines.

### Step 1: Open VM Network Settings

1. Select `WinServer-DC01`.
2. Click **Settings**.
3. Select **Network**.
4. Open **Adapter 1**.

Configure:

| Setting                | Value           |
| ---------------------- | --------------- |
| Enable Network Adapter | Enabled         |
| Attached To            | NAT Network     |
| Name                   | CEH-Lab-Network |
| Cable Connected        | Enabled         |

5. Click **OK**.

**Expected Result:** Windows Server should be connected to the common lab network.

## 7. Install Windows Server 2022

### Step 1: Start the VM

1. Select `WinServer-DC01`.
2. Click **Start**.
3. Wait for the Windows Server installation screen.

### Step 2: Select Installation Options

1. Select the required language.
2. Select the time and currency format.
3. Select the keyboard layout.
4. Click **Next**.
5. Click **Install now**.

### Step 3: Select Operating System Edition

Select the appropriate Windows Server 2022 Standard Evaluation edition with Desktop Experience.

**Important:** Desktop Experience provides a graphical interface, which is convenient for this beginner-friendly lab.

### Step 4: Accept the License Terms

1. Read and accept the license terms.
2. Click **Next**.

### Step 5: Select Installation Type

Select:

```text
Custom: Install Microsoft Server Operating System only
```

### Step 6: Select the Virtual Disk

1. Select the available 50 GB virtual disk.
2. Click **Next**.
3. Wait for installation to complete.

### Step 7: Set Administrator Password

1. Wait for Windows Server to finish installation.
2. Set the local Administrator password.
3. Complete the initial login process.

**Expected Result:** Windows Server 2022 should start successfully and display Server Manager or the Windows Server desktop.

## 8. Initial Server Configuration

### Step 1: Verify Windows Server Version

Open PowerShell as Administrator.

Execute:

```powershell
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber
```

Verified configuration:

```text
Windows Server 2022 Standard Evaluation
Windows Version: 2009
OS Build: 20348
```

### Step 2: Configure the Computer Name

The server computer name should be:

```text
CEH-DC01
```

To check the current name:

```powershell
hostname
```

If a name change is required, use Server Manager → Local Server → Computer Name → Change.

Enter:

```text
CEH-DC01
```

Restart the server when Windows requests it.

Verify:

```powershell
hostname
```

**Expected Result:**

```text
CEH-DC01
```

### Step 3: Configure the Time Zone

The lab uses India Standard Time.

Open PowerShell as Administrator and execute:

```powershell
Set-TimeZone -Id "India Standard Time"
```

Verify:

```powershell
Get-TimeZone
```

Expected time zone:

```text
India Standard Time
UTC+05:30
```

### Step 4: Verify the Windows Server Edition

Execute:

```powershell
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber
```

**Expected Result:** Windows Server 2022 Standard Evaluation, build 20348.

## 9. Configure and Verify Network Settings

### Step 1: Open Network Configuration

1. Log in to Windows Server.
2. Open Server Manager.
3. Select **Local Server**.
4. Click the Ethernet connection.
5. Open the network adapter properties.

Alternatively, open PowerShell and execute:

```powershell
Get-NetIPConfiguration
```

### Step 2: Verify IPv4 Configuration

The server's verified network configuration is:

| Parameter        | Value         |
| ---------------- | ------------- |
| IPv4 Address     | 192.168.56.7  |
| Subnet Mask      | 255.255.255.0 |
| Default Gateway  | 192.168.56.1  |
| DNS Before AD DS | 192.168.0.1   |

Verify using:

```cmd
ipconfig /all
```

**Expected Result:** The Ethernet adapter should have the expected lab IP address and gateway.

### Step 3: Verify Network Adapter

Execute in PowerShell:

```powershell
Get-NetAdapter | Select-Object Name, MacAddress, Status
```

Verified adapter details:

```text
Name: Ethernet
MAC Address: 08-00-27-ED-BB-DF
Status: Up
```

## 10. Configure DHCP Reservation in VirtualBox

A DHCP reservation ensures that the Windows Server receives the same IP address from the VirtualBox NAT Network DHCP server.

### Step 1: Identify the Server MAC Address

The verified server MAC address is:

```text
08-00-27-ED-BB-DF
```

### Step 2: Open PowerShell on the Host Machine

On the physical Windows host, open PowerShell as Administrator.

Execute:

```powershell
& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" list dhcpservers
```

Identify the DHCP server associated with:

```text
CEH-Lab-Network
```

### Step 3: Create the DHCP Reservation

Execute:

```powershell
& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" dhcpserver modify --network="CEH-Lab-Network" --mac-address="08:00:27:ED:BB:DF" --fixed-address="192.168.56.7"
```

### Step 4: Verify the Reservation

Execute:

```powershell
& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" list dhcpservers
```

Verify that the configuration contains:

```text
MAC Address: 08-00-27-ED-BB-DF
Fixed Address: 192.168.56.7
```

### Step 5: Renew the Server IP Address

Inside Windows Server, open Command Prompt as Administrator.

Execute:

```cmd
ipconfig /release
ipconfig /renew
```

Verify:

```cmd
ipconfig
```

**Expected Result:** The server should receive:

```text
192.168.56.7
```

## 11. Verify Connectivity Between Lab Machines

All four machines must be connected to:

```text
CEH-Lab-Network
```

### Current Lab IP Address Table

| Machine             | IPv4 Address |
| ------------------- | ------------ |
| Kali Linux          | 192.168.56.3 |
| Metasploitable 2    | 192.168.56.4 |
| Windows 10 Client   | 192.168.56.6 |
| Windows Server 2022 | 192.168.56.7 |

### Step 1: Verify the Default Gateway

From Windows Server, execute:

```cmd
ping 192.168.56.1
```

### Step 2: Test Kali Connectivity

Execute:

```cmd
ping 192.168.56.x
```

### Step 3: Test Metasploitable 2 Connectivity

Execute:

```cmd
ping 192.168.56.x
```

### Step 4: Test Windows 10 Connectivity

Execute:

```cmd
ping 192.168.56.x
```

**Expected Result:** Successful replies should be received from the reachable lab machines.

**Note:** Windows Firewall may block inbound ICMP Echo Requests by default. In this lab, inbound ICMPv4 Echo Request rules were enabled on Windows Server and Windows 10 to permit ping testing. Keep Windows Firewall enabled.

## 12. Install Active Directory Domain Services

### Step 1: Open Server Manager

1. Log in to Windows Server.
2. Open **Server Manager**.
3. Click **Manage**.
4. Select **Add Roles and Features**.

### Step 2: Select Installation Type

Select:

```text
Role-based or feature-based installation
```

Click **Next**.

### Step 3: Select the Destination Server

Select:

```text
CEH-DC01
```

Click **Next**.

### Step 4: Select Server Roles

1. Locate **Active Directory Domain Services**.
2. Select the checkbox.
3. When prompted, click **Add Features**.
4. Click **Next** through the remaining wizard pages.
5. Review the installation selections.
6. Click **Install**.

**Expected Result:** The AD DS role should be installed successfully.

### Step 5: Verify the AD DS Role

In Server Manager, check the Roles and Server Groups section.

The AD DS role should be visible.

## 13. Promote the Server to a Domain Controller

\<text color="warning" weight="semibold" size="sm">Pending completion after the required restart\</text>

The planned Active Directory domain is:

```text
ceh.lab
```

### Step 1: Open the Promotion Wizard

1. Open Server Manager.
2. Click the notification flag in the top-right corner.
3. Select:

```text
Promote this server to a domain controller
```

### Step 2: Select Deployment Configuration

Select:

```text
Add a new forest
```

Enter the root domain:

```text
ceh.lab
```

Click **Next**.

### Step 3: Configure Domain Controller Options

Use the following configuration:

| Setting                     | Value               |
| --------------------------- | ------------------- |
| Forest Functional Level     | Windows Server 2016 |
| Domain Functional Level     | Windows Server 2016 |
| DNS Server                  | Selected            |
| Global Catalog              | Selected            |
| Read Only Domain Controller | Not Selected        |

Set the Directory Services Restore Mode (DSRM) password and store it securely.

**Security Note:** Never publish the DSRM password in this GitHub document.

### Step 4: DNS Options

Leave DNS delegation unchecked for this new private lab forest unless a separate parent DNS infrastructure requires it.

A delegation warning may appear because this is a new forest. Review the warning and continue if the remaining configuration is correct.

### Step 5: Additional Options

Verify the NetBIOS domain name:

```text
CEH
```

### Step 6: Database, Log and SYSVOL Paths

Use the default paths:

| Component | Path              |
| --------- | ----------------- |
| Database  | C:\Windows\NTDS   |
| Logs      | C:\Windows\NTDS   |
| SYSVOL    | C:\Windows\SYSVOL |

### Step 7: Review and Run Prerequisite Check

1. Review the configuration.
2. Click **Next**.
3. Wait for the prerequisite check.

If the prerequisite check passes, click **Install**.

The server will restart automatically during the domain controller promotion process.

**Expected Result:** Windows Server should become the first domain controller for the `ceh.lab` forest.

## 14. Configure and Verify DNS

\<text color="warning" weight="semibold" size="sm">Pending successful domain controller promotion\</text>

DNS is required for Active Directory domain discovery and authentication.

### Step 1: Configure the Server DNS Client

After DNS is installed and operational, configure the server to use its own DNS service.

Open PowerShell as Administrator:

```powershell
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 192.168.56.7
```

Verify:

```powershell
Get-DnsClientServerAddress -AddressFamily IPv4
```

Expected DNS server:

```text
192.168.56.7
```

### Step 2: Verify DNS Resolution

Execute:

```cmd
nslookup ceh.lab
```

Then:

```cmd
nslookup CEH-DC01.ceh.lab
```

The domain and server hostname should resolve through the AD DNS service.

### Step 3: Verify DNS Manager

1. Open Server Manager.
2. Select **Tools**.
3. Open **DNS**.
4. Expand the server name.
5. Expand **Forward Lookup Zones**.
6. Verify the `ceh.lab` zone.

The domain zone should contain the relevant host and Active Directory service records after successful registration.

### Step 4: Verify DNS Forwarders

If external DNS resolution is required:

1. Open DNS Manager.
2. Right-click the server name.
3. Select **Properties**.
4. Open the **Forwarders** tab.
5. Configure the lab's upstream DNS server if appropriate.

The upstream DNS address used in the initial lab network was:

```text
192.168.0.1
```

Verify that this address is reachable and provides DNS service before configuring it as a forwarder.

## 15. Verify Domain Controller Health

\<text color="warning" weight="semibold" size="sm">Pending successful domain controller promotion\</text>

### Step 1: Verify Domain Membership

Execute:

```powershell
Get-ComputerInfo | Select-Object CsDomain, CsPartOfDomain
```

Expected domain:

```text
ceh.lab
```

Expected domain membership:

```text
True
```

### Step 2: Verify Important Services

Execute:

```powershell
Get-Service NTDS, DNS, Netlogon, KDC, DFSR
```

Verify that the required services are running.

### Step 3: Verify SYSVOL and NETLOGON Shares

Execute:

```cmd
net share
```

Verify that the following shares are present:

```text
SYSVOL
NETLOGON
```

### Step 4: Run Domain Controller Diagnostics

Execute:

```cmd
dcdiag /v
```

For DNS-specific diagnostics:

```cmd
dcdiag /test:dns /v
```

Review the results and investigate any failed tests.

## 16. Troubleshooting

| Problem                                    | Cause                                         | Solution                                                       |
| ------------------------------------------ | --------------------------------------------- | -------------------------------------------------------------- |
| Server receives an unexpected IP           | DHCP reservation missing or incorrect         | Verify the server MAC address and reservation                  |
| Cannot ping another machine                | Firewall blocks ICMP or network mismatch      | Check the NAT Network and inbound ICMP rules                   |
| AD DS promotion prerequisite check fails   | Pending role change requires restart          | Restart Windows Server and retry the wizard                    |
| DNS resolution fails after promotion       | DNS service or client DNS configuration issue | Verify DNS service and configure the server to use its own DNS |
| Domain name cannot be resolved             | DNS zone or records not registered            | Check DNS Manager and run DNS diagnostics                      |
| Domain controller services are not running | Promotion or service startup issue            | Review service status and Server Manager notifications         |
| SYSVOL or NETLOGON shares are missing      | Domain controller initialization issue        | Check DFSR and domain controller health                        |

## 17. Security Guidelines

1. Keep Windows Defender Firewall enabled.
2. Use strong passwords for Administrator and DSRM accounts.
3. Do not publish passwords or sensitive configuration information to GitHub.
4. Keep the vulnerable lab machines on the intended lab network.
5. Avoid using Bridged Adapter for vulnerable lab machines.
6. Do not expose domain controller services to the public internet.
7. Take VirtualBox snapshots before major configuration changes.
8. Use the domain only for authorized lab exercises.

**Network Note:** The NAT Network allows the lab machines to communicate and may permit outbound internet access. It is not a completely isolated network.

## 18. Final Verification Checklist

### Windows Server Setup

- [x] Windows Server 2022 installed.
- [x] VM resources configured.
- [x] Computer name configured as `CEH-DC01`.
- [x] India Standard Time configured.
- [x] Connected to `CEH-Lab-Network`.
- [x] Server IP configured through DHCP reservation.
- [x] Server IP verified as `192.168.56.7`.
- [x] Basic connectivity with lab machines verified.
- [x] AD DS role installed.

### Active Directory and DNS

- [ ] Server restarted after pending role change.
- [ ] Domain controller promotion completed.
- [ ] `ceh.lab` forest created.
- [ ] DNS service verified.
- [ ] Server DNS client configured to use `192.168.56.7`.
- [ ] Domain and server DNS resolution verified.
- [ ] SYSVOL and NETLOGON shares verified.
- [ ] Domain controller diagnostics completed.

## 19. Conclusion

Windows Server 2022 has been installed and configured as the server machine in the CEH Practical Lab.

The server is connected to the common `CEH-Lab-Network` and has the reserved IP address `192.168.56.7`.

The AD DS role has been installed. Domain controller promotion and DNS validation remain pending until the required restart and successful completion of the promotion wizard.

After these tasks are completed, the server will provide the Active Directory and DNS foundation required for joining the Windows 10 client to the `ceh.lab` domain.

---

**Document:** SOP-04
**Project:** CEH Practical Lab
**Platform:** Oracle VirtualBox
**Operating System:** Windows Server 2022 Standard Evaluation
**Server Name:** CEH-DC01
**Planned Domain:** ceh.lab
**Status:** In Progress

