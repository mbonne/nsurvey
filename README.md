# nsurvey

# Purpose:
Automate simple network survey from macOS.

# What does this script do:
Runs a network survey against your connected network, or against a list of subnets(CIDR notation) in txt file.
If using a list of subnets. It can be implied you're testing inter subnet/vlan routing or ACLs.
nmap is using the following command against each IP address specified:

`$ nmap -T4 -F --traceroute "$targetIP"`

(Basic and quick port scan, traceroute, will guess the Manufacturer from MAC OUI)

example contents of your .txt file:
10.0.0.0/8
172.16.0.0/14
192.168.0.0/16
(I wouldn't recomend using these as your targets, unless you have some time spare...)

# Runtime:
- Get local host info
  Hostname:
  Active NIC:
  MAC Address:
  Connected SSID: If there is any
  LAN IP:
  Subnetmask:
  CIDR block:
  Gateway:
  DHCP Server:
  DNS: Listed in order

- Internet Connectivity and Speedtest
  Thanks to: curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python -

- Shows the DHCP Packet info

- Checks if .txt specified when running script. Will scan subnets in .txt file, or guessed local network
  (gateway + CIDR block)
  will use nmap if found, or simple arp and ping scan on current broadcast domain(can't use list of subnets if no nmap found.)

# Before using this script:
- Make sure you have permission to scan network.
- Install and learn how to use nmap.


# How to use script:
Run script on its own to scan your current connected network.

`$ sudo ./nsurvey`

or

`$ sudo ./nsurvey /path/to/file/listOfSubnets.txt`

- If no file specified, script will attempt to fudge the 'subnet to scan' using the gateway and bit prefix of current subnet to target network for nmap. This is a bit of hack, open to suggestions for more accurate parsing of connected subnet.

Note about Filtered Ports: https://security.stackexchange.com/questions/9322/nmap-scan-shows-ports-are-filtered-but-nessus-scan-shows-no-result
"Unless you've got nmap configured not to perform host discovery (-PN or -PN --send-ip on the LAN), if it is indicating that all ports are filtered, then the host is up, but the firewall on that host is dropping traffic to all the scanned ports.

Note that a default nmap scan does not probe all ports. It only scans 1000 TCP ports. If you want to check for any services, you'll want to check all 65535 TCP ports and all 65535 UDP ports.

Also, to be precise, but when the port scan says a port is filtered, that doesn't mean that there is no service running on that port. It's possible that the host's firewall has rules that are denying access to the IP from which you're running the scan, but there may be other IPs which are allowed to access that service.

If the port scan reports that a port is closed, that's more definitive that there's no service listening on that port."
