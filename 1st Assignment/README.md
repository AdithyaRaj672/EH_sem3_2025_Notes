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
