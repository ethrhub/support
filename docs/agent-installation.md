# Agent Installation Guide

This guide provides detailed instructions for installing and configuring Ethr agents on various platforms.

## System Requirements

### Minimum Requirements
- **CPU**: 1 core
- **RAM**: 512 MB
- **Disk**: 50 MB free space
- **Network**: Internet connectivity to EthrHub server

### Recommended Requirements
- **CPU**: 2+ cores
- **RAM**: 1 GB
- **Disk**: 100 MB free space
- **Network**: Low-latency connection to EthrHub

### Supported Platforms
- Linux (x64, ARM64)
- macOS (x64, ARM64/Apple Silicon)
- Windows (x64)

## Installation

### Linux

#### Ubuntu/Debian
```bash
# Download the latest release
wget https://github.com/microsoft/ethr/releases/latest/download/ethr_linux.zip

# Extract
unzip ethr_linux.zip

# Make executable
chmod +x ethr

# Optional: Move to system path
sudo mv ethr /usr/local/bin/

# Verify installation
ethr --version
```

#### RHEL/CentOS/Fedora
```bash
# Download
curl -L https://github.com/microsoft/ethr/releases/latest/download/ethr_linux.zip -o ethr_linux.zip

# Extract and install
unzip ethr_linux.zip
chmod +x ethr
sudo mv ethr /usr/local/bin/
```

### macOS

#### Using Homebrew (Recommended)
```bash
# If Ethr is available via Homebrew
brew install ethr
```

#### Manual Installation
```bash
# Download
curl -L https://github.com/microsoft/ethr/releases/latest/download/ethr_darwin.zip -o ethr_darwin.zip

# Extract
unzip ethr_darwin.zip

# Make executable
chmod +x ethr

# Move to system path
sudo mv ethr /usr/local/bin/

# For Apple Silicon Macs, use the ARM64 version if available
```

### Windows

#### Using PowerShell
```powershell
# Download
Invoke-WebRequest -Uri "https://github.com/microsoft/ethr/releases/latest/download/ethr_windows.zip" -OutFile "ethr_windows.zip"

# Extract (requires PowerShell 5.0+)
Expand-Archive -Path ethr_windows.zip -DestinationPath C:\ethr

# Add to PATH (optional, run as Administrator)
$env:Path += ";C:\ethr"
setx PATH "$env:Path;C:\ethr" /M
```

#### Manual Installation
1. Download `ethr_windows.zip` from the [releases page](https://github.com/microsoft/ethr/releases/latest)
2. Extract to a folder (e.g., `C:\ethr`)
3. Add the folder to your PATH environment variable (optional)

## Running the Agent

### Basic Usage

Connect your agent to EthrHub:

```bash
./ethr -hub https://www.ethrhub.com
```

### Device Authorization

When you first run the agent, you'll need to authorize it:

1. The agent will display a verification URL and code
2. Visit the URL in your browser
3. Enter the code to authorize the agent
4. The agent will automatically connect once authorized

### Advanced Options

#### Use IPv6
```bash
./ethr -hub https://www.ethrhub.com -6
```

#### Enable Debug Logging
```bash
./ethr -hub https://www.ethrhub.com -debug
```

#### Specify Network Interface
```bash
./ethr -hub https://www.ethrhub.com -ip 192.168.1.100
```

**Note:** The `-port` parameter is only used in server/client mode (not hub mode) to specify the server's listening port or destination port. In hub mode, agents automatically use ephemeral local ports.

## Running as a Service

For production deployments, run the agent as a system service to ensure it starts automatically.

### Linux (systemd)

Create a service file:

```bash
sudo nano /etc/systemd/system/ethrhub-agent.service
```

Add the following content:

```ini
[Unit]
Description=EthrHub Agent
After=network.target

[Service]
Type=simple
User=ethrhub
ExecStart=/usr/local/bin/ethr -hub https://www.ethrhub.com
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Enable and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable ethrhub-agent
sudo systemctl start ethrhub-agent

# Check status
sudo systemctl status ethrhub-agent

# View logs
sudo journalctl -u ethrhub-agent -f
```

### macOS (launchd)

Create a launch agent plist:

```bash
sudo nano /Library/LaunchDaemons/com.ethrhub.agent.plist
```

Add the following content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.ethrhub.agent</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/bin/ethr</string>
        <string>-hub</string>
        <string>https://www.ethrhub.com</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
    <key>StandardOutPath</key>
    <string>/var/log/ethrhub-agent.log</string>
    <key>StandardErrorPath</key>
    <string>/var/log/ethrhub-agent-error.log</string>
</dict>
</plist>
```

Load and start:

```bash
sudo launchctl load /Library/LaunchDaemons/com.ethrhub.agent.plist
sudo launchctl start com.ethrhub.agent
```

### Windows (Service)

Use [NSSM](https://nssm.cc/) (Non-Sucking Service Manager):

```powershell
# Download and install NSSM
# Then install the service
nssm install EthrHubAgent "C:\ethr\ethr.exe" "-hub https://www.ethrhub.com"
nssm start EthrHubAgent
```

## Firewall Configuration

### Required Outbound Connections
- **HTTPS (443)**: To www.ethrhub.com for control plane
- **Custom Ports**: For test traffic between agents

### Test Traffic Ports
By default, Ethr uses:
- **TCP/UDP 8888**: Default test port
- **TCP 9999**: Control port (if custom specified)

Make sure firewall rules allow:
1. Outbound HTTPS to EthrHub server
2. Bidirectional traffic on test ports between agents

### Example: Linux iptables
```bash
# Allow outbound HTTPS
sudo iptables -A OUTPUT -p tcp --dport 443 -j ACCEPT

# Allow Ethr test traffic
sudo iptables -A INPUT -p tcp --dport 8888 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 8888 -j ACCEPT
```

### Example: Windows Firewall
```powershell
# Allow Ethr through firewall
New-NetFirewallRule -DisplayName "EthrHub Agent" -Direction Inbound -Program "C:\ethr\ethr.exe" -Action Allow
```

## Token Management

Agent tokens are stored locally:

- **Linux/macOS**: `~/.ethr_tokens/`
- **Windows**: `%USERPROFILE%\.ethr_tokens\`

Tokens are automatically refreshed and persist across agent restarts.

### Revoking Access

To revoke an agent's access:
1. Go to your EthrHub dashboard
2. Navigate to the agent list
3. Click the agent you want to remove
4. Click "Remove Agent"
5. Delete the token file on the agent machine

## Troubleshooting

### Agent Won't Connect
- Verify internet connectivity
- Check that www.ethrhub.com is accessible
- Ensure firewall allows outbound HTTPS
- Try running with `-debug` flag for more information

### "Authentication Required" Loop
- Delete old tokens: `rm -rf ~/.ethr_tokens/`
- Rerun the agent and complete device authorization
- Check that system time is correct (OAuth requires accurate time)

### Performance Issues
- Check CPU and memory usage
- Ensure network latency to EthrHub is acceptable
- Verify no bandwidth limitations on the connection
- Use `-debug` to identify bottlenecks

## Next Steps

- [Run your first test](running-tests.md)
- [Understand agent status indicators](faq.md#agent-status)
- [Configure advanced settings](api-reference.md)
- [Get help in discussions](https://github.com/ethrhub/hub/discussions)
