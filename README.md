# ipv4guard

A Cobalt Strike Aggressor Script that _attempts_ to aid a Cobalt Strike operator from being executing commands on non-whitelisted / off-target/ unapproved IPv4 target addresses.

## How It Works
After an operator adds an IP address to the monitoring list, the Cobalt Strike Aggressor Script monitors the `beacon_input` hook for IPv4 that matches addresses that take the proper action and halts the action when it detects a match.


# Usage

**Right-Click Beacon in Session Window -> Select "IPv4 Guard".**
![S1](Screenshots/S1.png)

**Input CIDR IP (like 192.168.1.1/30). Select "Add" as the action. Click Perform.**  
![S2](Screenshots/S2.png)

![S2-1](Screenshots/S2-1.png)  
[10/19 11:43:47] [*] Assuming CIDR address.  
[10/19 11:41:41] [+] Add action complete.  

**Right-Click Beacon in Session Window -> Select "IPv4 Guard".** 
![S1](Screenshots/S1.png)

**Select "List" as the action. IP is ignored. Click Perform.**  
![S3](Screenshots/S3.png)

![S3-1](Screenshots/S3-1.png)  
[10/19 11:43:47] [*] Assuming CIDR address.  
[10/19 11:43:47] [*] Hosts: @('192.168.1.1', '192.168.1.2', '192.168.1.3', '192.168.1.4')  
[10/19 11:43:47] [+] List action complete.  

**Enter into the Beacon Console a command with the off-limit IP.**

[10/19 11:45:16] [*] Tasked beacon to run: 192.168.1.1  
[10/19 11:45:16] [*]  Line -> shell 192.168.1.1  
[10/19 11:45:16] [*] IP match - 192.168.1.13 - found in IPGuard.  
[10/19 11:45:16] [*] Verify your entered command.  
![S5](Screenshots/S5.png)


---

Additional helper functions inside a beacon window can run and act as shorthand for determining the number of hosts in a CIDR or listing out all IPs in that range

Examples:

**Get number of hosts in a CIDR**

[10/19 11:38:46] beacon> ipv4check 192.168.1.1/24 hostcount  
[10/19 11:38:46] [*] Assuming CIDR address.  
[10/19 11:38:46] [*] Hostcount: 254  

**List hosts**

[10/19 11:39:55] beacon> ipv4check 192.168.1.1/30 hosts  
[10/19 11:39:55] [*] Assuming CIDR address.  
[10/19 11:39:55] [*] Hosts: 192.168.1.1, 192.168.1.2  


---
# Caveat and Limitations

- If an operator is already operating from a non-whitelisted target, one could possibly run commands from that target and the CNA script can not determine its origination point.
- Similarily, operators should still be aware of where theyre actually operating from (ie be aware if your socks proxying, tunneling into different VLANs, targeting different RFC 1918 IP addresses)
- Higher memory consumption on large CIDR ( aruond /12 or lower) blocks. Thusly not suggested for low memory systems.

---
# TODOs

- Investigate adding hostname-based detection/prevention.
- Investigate common systools output parsing

---
# References

- Net::IP::Match::Regexp
