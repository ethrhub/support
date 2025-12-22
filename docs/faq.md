# Frequently Asked Questions

## General

### What is EthrHub?
EthrHub is a cloud-based platform for network performance measurement. It provides a centralized dashboard for managing distributed Ethr agents and running network tests.

### How is EthrHub different from Ethr?
Ethr is Microsoft's command-line tool for network testing. EthrHub adds:
- Web-based dashboard
- Multi-user support
- Centralized management
- Historical data
- OAuth authentication
- Scheduling (coming soon)

### Is EthrHub free?
Yes! We offer a free Basic plan with essential features. Premium plans are available for advanced needs.

### Do I need to install anything?
You only need to download the Ethr agent binary and run it on machines where you want to measure performance. The dashboard is entirely web-based.

## Agents

### How many agents can I connect?
- **Basic Plan**: Up to 10 agents
- **Premium Plan**: Unlimited agents

### What do the agent statuses mean?
- **Connected**: Agent is online and ready
- **Busy**: Agent is currently running a test
- **Disconnected**: Agent is offline or unreachable

### Can agents connect through a firewall?
Yes. Agents only need outbound HTTPS (443) access to www.ethrhub.com. Test traffic flows directly between agents, so those ports must be accessible between agents.

### How do I remove an agent?
1. Go to your dashboard
2. Find the agent in the list
3. Click the delete/remove button
4. Confirm removal
5. Optionally delete token files on the agent machine

### Can I run multiple agents on the same machine?
Yes! When running multiple agents in hub mode on the same machine, they automatically use different ephemeral local ports, so no special configuration is needed. Simply run each agent separately:
```bash
./ethr -hub https://www.ethrhub.com
./ethr -hub https://www.ethrhub.com
```

**Note:** The `-port` parameter is only needed in server/client mode to specify the server's listening port, not for hub mode.

### Where are agent tokens stored?
- **Linux/macOS**: `~/.ethr_tokens/`
- **Windows**: `%USERPROFILE%\.ethr_tokens\`

Tokens are automatically managed and refreshed by the agent.

## Tests

### How long does a test take?
You can configure test duration from 1 to 300 seconds. Default is 10 seconds.

### Can I run multiple tests simultaneously?
Yes! You can run tests on different agent pairs at the same time. Basic plan allows up to 10 concurrent tests.

### Why did my test fail?
Common reasons:
- Agents not connected
- Firewall blocking test traffic
- Agent busy with another test
- Network connectivity issues
- Insufficient resources on agent

### Can agents test themselves?
No, tests must be between two different agents. Loopback tests are not supported.

### What's the difference between TCP and UDP tests?
- **TCP**: Reliable, connection-oriented. Better for bandwidth tests.
- **UDP**: Faster, connectionless. Better for latency tests.

### How accurate are the results?
Very accurate. Ethr uses proven methodologies similar to iperf. However, results can vary based on:
- Network conditions
- Agent resources
- Background traffic
- Test parameters

### Can I export test results?
Yes! Click on any test to view details, then use the export button to download data in CSV or JSON format.

## Authentication & Security

### How do I sign in?
EthrHub uses OAuth 2.0. Sign in with your existing Google, GitHub, Microsoft, or other OAuth provider account.

### How are agents authenticated?
Agents use OAuth device authorization flow. When you start an agent, it provides a code to enter in your browser, linking it to your account.

### Is my data secure?
Yes! All communication uses HTTPS/TLS encryption. Tokens are stored securely and never transmitted in plaintext.

### Can I share my account?
Each user should have their own account. Premium plans support team features with role-based access.

### How do I reset my password?
EthrHub uses OAuth, so password management is handled by your OAuth provider (Google, GitHub, etc.).

## Billing & Plans

### What's included in the Basic plan?
- Up to 5 agents
- 50 tests per day
- 30 days of history
- Community support

### What's included in the Premium plan?
- Up to 100 agents
- 1,000 tests per day
- Unlimited history
- Priority support
- Advanced analytics
- API access
- Scheduled tests

### Can I upgrade anytime?
Yes! Upgrade from your account settings. Changes take effect immediately.

### Do you offer refunds?
Yes, we offer a 30-day money-back guarantee for Premium plans.

### Can I cancel my subscription?
Yes, cancel anytime from your account settings. You'll retain access until the end of your billing period.

## Troubleshooting

### Agent shows as "Disconnected"
Troubleshooting steps:
1. Check if the agent process is running
2. Verify network connectivity to www.ethrhub.com
3. Check firewall allows outbound HTTPS (443)
4. Try restarting the agent
5. Check agent logs with `-debug` flag

### Tests always fail
Check:
1. Both agents are "Connected" status
2. Agents can reach each other (network connectivity)
3. Firewall allows traffic on test ports (default 8888)
4. No other tests running on the same agents
5. Agents have sufficient CPU/memory

### I can't log in
Try:
1. Clear browser cookies and cache
2. Try a different browser
3. Verify your OAuth provider is working
4. Check if you're using the correct email
5. Contact support@ethrhub.com if issues persist

### Results seem wrong
Consider:
1. Network conditions during the test
2. Background traffic on the network
3. Agent resource availability
4. Test parameter configuration
5. Try running multiple tests and averaging results

### Agent authorization fails
Solutions:
1. Delete old tokens: `rm -rf ~/.ethr_tokens/`
2. Verify system time is correct (OAuth requires accurate time)
3. Check network connectivity during authorization
4. Try using a different browser
5. Ensure you're logged in to www.ethrhub.com

## Technical Questions

### What ports does EthrHub use?
- **Control Plane**: HTTPS (443) to www.ethrhub.com
- **Test Traffic**: Configurable, default TCP/UDP 8888

### Can I use EthrHub on a private network?
Yes, but agents need outbound internet access to reach www.ethrhub.com for control. Test traffic can remain on your private network.

### Does EthrHub support IPv6?
Yes! Use the `-6` flag when starting agents:
```bash
./ethr -hub https://www.ethrhub.com -6
```

### What happens to my data if I cancel?
You have 30 days to export your data after cancellation. After that, data is permanently deleted per our privacy policy.

### Is there an SLA?
Premium plans include a 99.9% uptime SLA. See our Terms of Service for details.

## Getting More Help

### Where can I find more documentation?
- [Getting Started Guide](getting-started.md)
- [Agent Installation](agent-installation.md)
- [Running Tests](running-tests.md)
- [Troubleshooting](troubleshooting.md)

### How do I report a bug?
[Open an issue on GitHub](https://github.com/ethrhub/hub/issues/new?template=bug_report.md)

### How do I request a feature?
[Submit a feature request](https://github.com/ethrhub/hub/issues/new?template=feature_request.md)

### Where can I ask questions?
[Visit our Discussions forum](https://github.com/ethrhub/hub/discussions)

### How do I contact support?
- **Email**: support@ethrhub.com
- **Twitter**: [@ethrhub](https://twitter.com/ethrhub)
- **Response Time**: 24-48 hours (Basic), 4-8 hours (Premium)
