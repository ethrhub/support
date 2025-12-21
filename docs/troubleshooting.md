# Troubleshooting Guide

This guide helps you resolve common issues with EthrHub.

## Common Issues

### Authentication & Login

#### Cannot Log In
**Symptoms:** Login fails or redirects back to login page

**Solutions:**
1. Clear your browser cookies and cache
2. Try a different browser or incognito/private mode
3. Verify you're using a supported OAuth provider (Google, GitHub, Microsoft)
4. Check that JavaScript is enabled in your browser
5. Disable browser extensions that might interfere with authentication

#### Session Expires Quickly
**Symptoms:** Logged out frequently

**Solutions:**
1. Check your browser's cookie settings
2. Ensure cookies are enabled for ethrhub.com
3. Try not using private/incognito mode
4. Check if your browser is set to clear cookies on exit

### Agent Issues

#### Agent Shows as "Disconnected"
**Symptoms:** Agent appears offline in dashboard despite running

**Solutions:**
1. Verify the agent process is still running
2. Check network connectivity to ethrhub.com
3. Verify firewall allows outbound HTTPS (port 443)
4. Check agent logs for error messages
5. Restart the agent process
6. Re-authorize the agent if tokens expired

**Check agent status:**
```bash
ps aux | grep ethr
```

**Check network connectivity:**
```bash
curl -I https://www.ethrhub.com
```

#### Agent Authorization Fails
**Symptoms:** Cannot complete device authorization flow

**Solutions:**
1. Delete old tokens:
   ```bash
   rm -rf ~/.ethr_tokens/
   ```
2. Verify system time is correct (OAuth requires accurate time)
3. Check network connectivity during authorization
4. Try a different browser for authorization
5. Ensure you're logged into ethrhub.com
6. Clear browser cache and try again

#### Agent Crashes or Exits
**Symptoms:** Agent process stops unexpectedly

**Solutions:**
1. Run agent with debug flag to see detailed logs:
   ```bash
   ./ethr -hub https://www.ethrhub.com -debug
   ```
2. Check system resources (CPU, memory, disk space)
3. Verify no port conflicts (default port 8888)
4. Update to latest Ethr version
5. Check for OS-specific issues in agent logs

#### Multiple Agents on Same Machine
**Symptoms:** Second agent won't start or conflicts

**Solutions:**
When running multiple agents in hub mode on the same machine, they automatically use different ephemeral local ports. No special configuration is needed:
```bash
./ethr -hub https://www.ethrhub.com
./ethr -hub https://www.ethrhub.com
```

**Note:** The `-port` parameter is only relevant in server/client mode (when using `-s` or `-c` flags), not for hub mode. In hub mode, agents automatically handle port allocation.

### Test Issues

#### Test Fails to Start
**Symptoms:** Test shows "Failed" status immediately

**Solutions:**
1. Verify both agents show "Connected" status
2. Check agents can reach each other over the network
3. Verify firewall allows traffic on test ports (default 8888)
4. Ensure neither agent is busy with another test
5. Check that both agents are authorized by the same user
6. Try a different test type or simpler parameters

**Test network connectivity between agents:**
```bash
# On source agent machine, test connectivity to destination
ping destination-ip
telnet destination-ip 8888
```

#### Test Results Show Zero Throughput
**Symptoms:** Bandwidth test shows 0 Mbps

**Solutions:**
1. Check firewall rules between agents
2. Verify correct ports are open
3. Check for network equipment blocking traffic
4. Try UDP instead of TCP (or vice versa)
5. Verify no rate limiting on network path
6. Check agent CPU isn't maxed out

#### Inconsistent Test Results
**Symptoms:** Results vary wildly between runs

**Solutions:**
1. Check for background network traffic during tests
2. Verify agents have sufficient CPU/memory
3. Run longer duration tests (30-60 seconds)
4. Run multiple tests and average results
5. Test at different times of day
6. Check for competing traffic on network

#### High Latency Results
**Symptoms:** Latency much higher than expected

**Solutions:**
1. Check network path (traceroute/tracert)
2. Verify no VPN or proxy in the path
3. Check for network congestion
4. Test during off-peak hours
5. Compare with other tools (ping)
6. Check for geographical distance between agents

### Dashboard Issues

#### Dashboard Loads Slowly
**Symptoms:** Long loading times or timeouts

**Solutions:**
1. Check your internet connection speed
2. Try a different browser
3. Clear browser cache
4. Disable browser extensions
5. Check if ethrhub.com is experiencing issues

#### Data Not Updating
**Symptoms:** Dashboard shows stale information

**Solutions:**
1. Refresh the page (F5 or Cmd/Ctrl+R)
2. Clear browser cache
3. Check browser console for errors (F12)
4. Verify JavaScript is enabled
5. Try a different browser

#### Cannot Create Test
**Symptoms:** "New Test" button doesn't work

**Solutions:**
1. Verify you have at least 2 connected agents
2. Check you haven't hit test limits (Basic plan: 100/day)
3. Refresh the page
4. Check browser console for errors
5. Try a different browser

### Network & Firewall

#### Required Firewall Rules

**Outbound (from agents to EthrHub):**
- HTTPS (443) to www.ethrhub.com

**Between Agents (for tests):**
- TCP/UDP port 8888 (or custom port specified)
- Bidirectional traffic required

**Example iptables rules (Linux):**
```bash
# Allow outbound HTTPS
sudo iptables -A OUTPUT -p tcp --dport 443 -j ACCEPT

# Allow Ethr test traffic
sudo iptables -A INPUT -p tcp --dport 8888 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 8888 -j ACCEPT
```

**Example Windows Firewall:**
```powershell
New-NetFirewallRule -DisplayName "EthrHub Agent" `
  -Direction Inbound `
  -Program "C:\path\to\ethr.exe" `
  -Action Allow
```

#### Cannot Connect Through Corporate Firewall
**Symptoms:** Agent can't reach ethrhub.com

**Solutions:**
1. Check if HTTPS outbound is blocked
2. Verify no SSL inspection breaking connections
3. Ask IT to whitelist ethrhub.com
4. Check proxy settings
5. Use proxy if required by your network

### Performance Issues

#### Agent Using High CPU
**Symptoms:** Agent process using excessive CPU

**Solutions:**
1. This is normal during active tests
2. Check if multiple tests running simultaneously
3. Reduce thread count in test parameters
4. Ensure agent machine has sufficient resources
5. Check for background processes competing for CPU

#### Agent Using High Memory
**Symptoms:** Agent consuming excessive RAM

**Solutions:**
1. Check buffer sizes in test parameters
2. Reduce number of concurrent connections
3. Restart agent periodically
4. Upgrade agent machine RAM if needed

### Data & History

#### Missing Historical Data
**Symptoms:** Old tests not showing in dashboard

**Solutions:**
1. Check your plan's retention period:
   - Basic: 30 days
   - Premium: Unlimited
2. Tests may have been deleted
3. Filter settings may be hiding tests
4. Try searching by date range

#### Cannot Export Data
**Symptoms:** Export function not working

**Solutions:**
1. Check browser popup blocker settings
2. Verify sufficient disk space
3. Try a different browser
4. Contact support if on Premium plan

## Error Messages

### "Authentication Required"
Agent needs to be authorized. Run the agent and follow the device authorization flow.

### "Agent Not Found"
Agent was removed or never registered. Re-run agent with hub parameter.

### "Permission Denied"
You don't have access to this agent or test. Verify you're logged in with the correct account.

### "Rate Limit Exceeded"
You've hit your plan's test limit. Wait for the limit to reset or upgrade to Premium.

### "Both Agents Must Be Connected"
One or both agents are offline. Check agent status in dashboard.

### "Invalid Test Parameters"
Test configuration is incorrect. Check duration, protocol, and other settings.

## Still Having Issues?

If you're still experiencing problems:

1. **Check System Status:** Visit our status page (if available)
2. **Search Issues:** Look for similar problems at https://github.com/ethrhub/ethrhub/issues
3. **Ask Community:** Post in https://github.com/ethrhub/ethrhub/discussions
4. **Contact Support:** Email support@ethrhub.com with:
   - Detailed description of the issue
   - Steps to reproduce
   - Agent IDs if relevant
   - Screenshots or logs
   - Your plan type (Basic/Premium)

## Getting Logs

### Agent Logs
Run agent with debug flag:
```bash
./ethr -hub https://www.ethrhub.com -debug > agent.log 2>&1
```

### Browser Console Logs
1. Press F12 to open Developer Tools
2. Go to Console tab
3. Reproduce the issue
4. Right-click and "Save as..." to export logs

## Useful Diagnostic Commands

### Check DNS Resolution
```bash
nslookup www.ethrhub.com
```

### Check HTTPS Connectivity
```bash
curl -v https://www.ethrhub.com
```

### Check Port Connectivity Between Agents
```bash
# On destination agent machine
nc -l 8888

# On source agent machine
nc destination-ip 8888
```

### Check System Time
```bash
date
# Should match actual time closely for OAuth to work
```

### View Agent Token Info
```bash
ls -la ~/.ethr_tokens/
cat ~/.ethr_tokens/www.ethrhub.com.json
```
