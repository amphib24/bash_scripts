# Nmap Network Scanner - Bash Reconnaissance Script

## Description
This is a Bash script I wrote during a live lab during my Pentest+ studies to gain a better understanding of how port scanners work under the hood by automating the scanning of commonly used TCP ports on a target host. The port scanner utilizes 
common Nmap flags and outputs to scan the selected host or range of hosts and output the results to a file.

## Disclaimer 
This was designed for educational purposes only and is not intended for illegal or malicious use of any kind.
## Features
   - Supports multiple Nmap Scan types
   - Allows scanning of a single IP or IP ranges
   - Custom port selection
   - Multiple Nmap output formats
   - Service enumeration and OS fingerprinting support
   - Command-line interaction

```bash
#!/bin/bash

#make list of available file output flags
declare -A file_flag=(
[-oA]="All output formats"
[-oN]="Normal output (.txt)"
[-oG]="grepable"
[-oX]="XML"
[-oS]="script kiddie"
[-oJ]="json"
)

#make list of available scan flags
declare -A scan_flag=(
[-sT]="TCP Connect"
[-sA]="ACK  scan(check if firewal is stateful)"
[-sS]="SYN scan(stealth)"
[-sX]="Christmas tree scan"
[-sF]="FIN scan"
[-sV]="Service Enum"
[-Pn]="Skip ping"
[-sn]="ping scan(host scan)"
[-A]="Agressive"
[-F]="Fast"
[-O]="OS fingerprinting"
)

#prompt for ip/ ip range
read -p "Enter ip or ip range you want scanned (use a common to separte multiple):"  ip_range

#convert ips into an array 
IFS=',' read -r -a ip_range <<< "$ip_range_input"

#prompt for file flag input and store in new variable
echo "Available output flags: ${!file_flag[@]}"
read -p "Please select an option from the list" chosen_flag

#prompt for scan flags and store in new variable/array
echo "Available scan options: ${!scan_flag[@]}"
read -p "Please slect an option from the list" -a chosen_scan

#prompt for output file path and store in variable
read -p "Enter output file path:" output_file

#prompt for ports to scan
read -p "Enter port or ports to scan:" -a port

#prepare port option for scan
port_option="-p${port}"

#prepare scan flags for scan
scan_flags="${choosen_scan[@]}"

#perform scan
nmap $scan_flags  "${ip_range[@]}" $port_option $chosen_flag "$output_file" 

echo "Scan completed. The output is located at: $output_file"
```
## Detection Considerations
From a SOC standpoint, indicators of scanning activity may include:
   - Sequential or high-volume port probes from a single source
   - Multiple scan types executed against the same host
   - Repeated connection attempts across a range of ports
   - SYN packet spikes in network traffic logs
This activity can be monitored and/ or detected using firewall logs, IDS/IPS, or SIEM solutions.
