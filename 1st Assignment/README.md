# 🔐 Simple Port Scanner in Bash

## 📋 Scenario:
You're building a lightweight security tool to check for **open ports** on a target machine. This script scans the **top 1000 ports** using Bash, without relying on `nmap`.

---

## 🛠️ Features

- Takes an IP address as input
- Scans ports `1` to `1000`
- Uses built-in Bash `/dev/tcp` for scanning
- Saves results to a timestamped log file
- Works on Linux (tested on Kali)

---

## 📜 Script

```bash
#!/bin/bash

# Ask user for the target IP address
read -p "Enter the target IP address: " ip

# Validate basic IP format
if [[ ! $ip =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}$ ]]; then
    echo "❌ Invalid IP address."
    exit 1
fi

# Get the current date for log file
timestamp=$(date +"%Y-%m-%d_%H-%M-%S")
log_file="scan_${timestamp}.log"

echo "🔍 Scanning top 1000 ports on $ip..."
echo "Scan started at $timestamp" > "$log_file"
echo "Target: $ip" >> "$log_file"
echo "--------------------------" >> "$log_file"

# Loop through ports 1 to 1000
for port in {1..1000}; do
    # Use nc (netcat) to check port
    timeout 0.5 bash -c "echo > /dev/tcp/$ip/$port" 2>/dev/null &&
    echo "Port $port is OPEN" | tee -a "$log_file"
done

echo "✅ Scan complete. Results saved to $log_file"
```
# Mini Port Scanner - Assignment Documentation

## Overview
This is a simple bash-based port scanner that checks the top 1000 ports on a target IP address. It's designed as a lightweight network reconnaissance tool for educational purposes.

## Features
- **IP Address Validation**: Basic regex validation for IPv4 addresses
- **Port Range Scanning**: Scans ports 1-1000 using TCP connections
- **Logging**: Automatically saves results to timestamped log files
- **Real-time Output**: Shows open ports as they're discovered
- **Timeout Protection**: Uses 0.5-second timeout to prevent hanging

## Code Structure

### 1. User Input & Validation
```bash
read -p "Enter the target IP address: " ip
if [[ ! $ip =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}$ ]]; then
    echo "❌ Invalid IP address."
    exit 1
fi
```
- Prompts user for target IP address
- Validates IP format using regex pattern
- Exits with error code 1 if invalid

### 2. Log File Setup
```bash
timestamp=$(date +"%Y-%m-%d_%H-%M-%S")
log_file="scan_${timestamp}.log"
```
- Creates unique log filename with timestamp
- Format: `scan_YYYY-MM-DD_HH-MM-SS.log`

### 3. Port Scanning Loop
```bash
for port in {1..1000}; do
    timeout 0.5 bash -c "echo > /dev/tcp/$ip/$port" 2>/dev/null &&
    echo "Port $port is OPEN" | tee -a "$log_file"
done
```
- Iterates through ports 1-1000
- Uses `/dev/tcp/` pseudo-device for TCP connections
- Suppresses error output with `2>/dev/null`
- Uses `tee` to display and log results simultaneously

## Technical Implementation

### Connection Method
The scanner uses bash's built-in `/dev/tcp/` feature:
- `/dev/tcp/host/port` - Opens TCP connection
- `echo >` - Attempts to write to the connection
- `timeout 0.5` - Limits connection attempt to 0.5 seconds

### Error Handling
- **Invalid IP**: Script exits with error message
- **Connection failures**: Suppressed with `2>/dev/null`
- **Timeouts**: Prevented with `timeout` command

### Output Format
- **Console**: Real-time display of open ports
- **Log file**: Complete scan record with metadata

## Usage Instructions

### Prerequisites
- Bash shell (Linux/macOS/WSL)
- Network connectivity to target
- Permission to make outbound connections

### Running the Scanner
1. Make the script executable:
   ```bash
   chmod +x port_scanner.sh
   ```

2. Run the scanner:
   ```bash
   ./port_scanner.sh
   ```

3. Enter target IP when prompted:
   ```
   Enter the target IP address: 192.168.1.1
   ```

### Example Output
```
🔍 Scanning top 1000 ports on 192.168.1.1...
Port 22 is OPEN
Port 80 is OPEN
Port 443 is OPEN
✅ Scan complete. Results saved to scan_2024-01-15_14-30-25.log
```

## Log File Format
```
Scan started at 2024-01-15_14-30-25
Target: 192.168.1.1
--------------------------
Port 22 is OPEN
Port 80 is OPEN
Port 443 is OPEN
```

## Common Open Ports
| Port | Service | Description |
|------|---------|-------------|
| 21 | FTP | File Transfer Protocol |
| 22 | SSH | Secure Shell |
| 23 | Telnet | Remote terminal |
| 25 | SMTP | Email transmission |
| 53 | DNS | Domain Name System |
| 80 | HTTP | Web traffic |
| 110 | POP3 | Email retrieval |
| 143 | IMAP | Email access |
| 443 | HTTPS | Secure web traffic |
| 993 | IMAPS | Secure IMAP |
| 995 | POP3S | Secure POP3 |

## Performance Considerations

### Speed vs Accuracy Trade-offs
- **Timeout Duration**: 0.5s balances speed vs reliability
- **Sequential Scanning**: Simple but slower than parallel
- **Port Range**: 1000 ports is manageable scan time

### Optimization Possibilities
- Parallel scanning with background processes
- Adjustable timeout values
- Custom port ranges
- Service detection capabilities

## Security & Ethical Considerations

### Legal Usage
- Only scan systems you own or have permission to test
- Respect network policies and terms of service
- Use for educational/authorized testing purposes only

### Detection Avoidance
- This scanner is easily detectable (no stealth features)
- Generates connection logs on target systems
- Sequential scanning creates obvious patterns

## Limitations

### Technical Limitations
- **No UDP scanning**: Only tests TCP ports
- **Basic validation**: Doesn't verify IP reachability
- **No service detection**: Only identifies open ports
- **Sequential only**: No parallel scanning
- **Limited range**: Only scans first 1000 ports

### Network Limitations
- Blocked by firewalls and intrusion detection systems
- May trigger security alerts
- Doesn't handle rate limiting
- No proxy or stealth capabilities

## Possible Enhancements

### Feature Additions
```bash
# Parallel scanning example
for port in {1..1000}; do
    (timeout 0.5 bash -c "echo > /dev/tcp/$ip/$port" 2>/dev/null &&
     echo "Port $port is OPEN") &
done
wait
```

### Error Handling Improvements
- Hostname resolution support
- Network connectivity checks
- Progress indicators
- Interrupt handling (Ctrl+C)

### Output Enhancements
- JSON output format
- Service banner grabbing
- Port categorization
- Scan statistics

## Assignment Considerations

### Learning Objectives
- Understanding TCP connections
- Bash scripting fundamentals
- Network reconnaissance basics
- File I/O operations
- Error handling patterns

### Extension Ideas
- Add UDP port scanning
- Implement service detection
- Create GUI interface
- Add scan result analysis
- Integrate with other tools

## Conclusion
This mini port scanner demonstrates fundamental network scanning concepts using basic bash features. While simple, it effectively illustrates TCP connection testing, file logging, and user interaction patterns commonly used in network security tools.

**Remember**: Always use responsibly and only on systems you're authorized to test!
