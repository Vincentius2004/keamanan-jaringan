Network Security Laboratory Manual: 14 Practical Sessions
Session 1, Introduction & Topology Setup (Kali VM ↔ Windows Host)
Objective: Set up VM topology, ensure bidirectional connectivity, and perform ICMP packet capture & analysis using Wireshark (Windows) & tcpdump (Kali).

Prerequisites & Topology

Host: Windows (with Wireshark installed)

VM: Kali Linux

VM Network: Host-Only (recommended) or Bridged

Example Host-Only IPs: Windows 192.168.56.1, Kali 192.168.56.101

Preparation: Configure Kali VM adapter:

VirtualBox → Settings → Network → Adapter 1 → Host-Only Adapter

VMware → VM Settings → Network Adapter → Host-only

Boot Windows & Kali. Verify Wireshark (Windows) & tcpdump (Kali) availability.

Steps

A. Identify IP addresses Kali:

 
ip addr show

# or:

hostname -I

Note IP (e.g., 192.168.56.101).

Windows (PowerShell/):

 
ipconfig

Note Host-Only IP (e.g., 192.168.56.1).

B. Test bidirectional connectivity Windows → Kali:

 
ping 192.168.56.101 -n 4

Kali → Windows:

 
ping -c 4 192.168.56.1

C. ICMP capture in Wireshark (Windows) Open Wireshark → select Host-Only interface → Start.

From Kali:

 
ping -c 5 192.168.56.1

Stop capture → apply filter:

icmp

or

ip.addr == 192.168.56.101 && ip.addr == 192.168.56.1

Analyze one frame: Ethernet (MAC src/dst), IP (src/dst), ICMP (Type 8 echo-request / Type 0 reply, Code 0).

D. Alternative capture in Kali

 
sudo tcpdump -i eth0 icmp -w icmp_cap.pcap

# perform ping from Windows

# stop with Ctrl+C

sudo tcpdump -r icmp_cap.pcap

(Optional: open icmp_cap.pcap in Wireshark Windows.)

Verification / Results

Successful bidirectional ping

Echo Request & Echo Reply visible in Wireshark

Able to explain MAC/IP/src/dst and ICMP type/code

Cleanup

Stop Wireshark/tcpdump

Save icmp_cap.pcap for reporting

Troubleshooting

Ping timeout → check adapter mode, Windows Firewall, interface status

Bring up interface in Kali:

 
sudo ip link set eth0 u
