Task 1 (what i did)
What I ran
1) Find your IP on Linux/macOS
css
hostname -I
Purpose: Retrieve the IP addresses assigned to the host (usually the primary interface on the local network).
Notes:
On some systems, this may print multiple addresses. The primary IPv4 address is usually the one used to reach other devices on the local network.
2) Quick discovery of live hosts on the /24 subnet
graphql
sudo nmap -sn 192.168.1.0/24 -oN live-hosts.txt
Purpose: Perform a ping sweep (no port scanning) to identify which hosts are online within the 192.168.1.0/24 network.
Output file: live-hosts.txt
Notes:
The -sn option tells Nmap to skip port scans and only check host availability.
Running with sudo ensures raw packets can be sent if required.
Example result (sanitized): a list of hosts marked as up, sometimes with MAC addresses if on the same broadcast domain.
3) SYN scan of discovered network, save output
sudo nmap -sS 192.168.1.0/24 -oA myscan
Purpose: Perform a fast, stealthy SYN scan (half-open scan) across the /24 network to identify open or filtered ports on hosts that are up.
Output files:
myscan.nmap (summary)
myscan.gnmap (grepable)
myscan.xml (XML)
Notes:
The -sS option is a common choice for quick, non-verbose port discovery.
This step builds a broader fingerprint of each host, useful for planning deeper assessment.
Depending on the environment, some ports may be shown as filtered or closed.
4) Detailed scan of a specific host
graphql
sudo nmap -sS -sV -O 192.168.1.42 -oN host-192-168-1-42.txt
Purpose: Conduct a thorough assessment of a single target:
-sS: Syn scan (port discovery)
-sV: Service version detection
-O: Operating System detection
Output file: host-192-168-1-42.txt
Notes:
The result provides open ports, associated services, version information, and an OS fingerprint (where detectable).
The IP address in the command is the target host; in this example, it has been sanitized to avoid exposing real addresses.
How to interpret the results
UP hosts: Indicate devices reachable on the network.
MAC Address: Helpful for identifying device manufacturers and for verifying if the host is on the same layer-2 network segment.
Port states:
open: Service listening on the port.
filtered: Firewall or packet filtering is interfering with probes.
closed: No service listening on the port.
ignored: Nmap may suppress issues or rely on the scan type.
Service/version OS info (from -sV and -O):
Provides software versions and operating system families, which aids in vulnerability assessment and asset inventory.
