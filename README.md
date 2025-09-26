# task1-install nmap and find your local IP Addresses
Install Nmap
Download from https://nmap.org/download.html

open nmap(GUI)
command:- nmap -sS 192.168.1.0/24
8 hosts are up after scanning local ip addresses.
Total 13 ports are open from 8 up hosts.

Potential security risks from open ports are:-
**1** 
192.168.1... – Zyxel router-53/tcp (DNS) – normal, but if misconfigured could be used for DNS amplification or hijacking. 80/tcp & 443/tcp (HTTP/HTTPS) – web admin interface. If left with default credentials or reachable from the WAN side, it’s a major risk (router takeover).22/tcp & 23/tcp filtered – looks blocked from your LAN. If those ports open up accidentally, Telnet/SSH access to the router is a high-value target.
**Mitigation**:
Use a strong admin password, disable remote/WAN access to the web UI, update firmware.

**2** 
192.168.1... – Windows laptop-135/139/445 (MSRPC/NetBIOS/SMB) – standard Windows file/printer sharing. Vulnerable to things like EternalBlue (SMBv1) if unpatched. Should be blocked on public networks. 
902/tcp (VMware authd) – if VMware Workstation/Player is installed, this port is open for remote management. Could allow code execution if an exploit exists.
912/tcp (VMware autoinstall / Apex-mesh) – also VMware-related. Similar risk.
7070/tcp (RealServer/RTSP) – old streaming protocol. If it’s not something you deliberately run, check what’s listening; some malware uses this port.
**Mitigation**:
Ensure Windows Update is current, SMBv1 disabled.
Restrict file/printer sharing to private network only.
Stop or firewall VMware services if not needed.
Run netstat -ano to see which process owns 902/912/7070.

**3**
192.168.1... – Unknown device (open 4321 & 5555)
5555/tcp – very commonly Android Debug Bridge (ADB) in TCP mode. If so, anyone on the LAN can get a shell on the device without authentication. Serious risk.
4321/tcp (rwhois) – rarely used on LAN. Could be a management or diagnostic service. Unknown risk without further probing.

**Mitigation**:
Identify the device (check ARP table, check connected devices in your router’s admin page). Disable ADB or any developer/debug mode. Update firmware if IoT.

**Note**: All IP addresses are hide because of security purpose.
