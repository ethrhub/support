# Getting Started with EthrHub

Welcome to EthrHub! This guide will help you get up and running with network performance monitoring in just a few minutes.

## Prerequisites

Before you begin, make sure you have:

- A modern web browser (Chrome, Firefox, Safari, or Edge)
- An OAuth provider account (Google, GitHub, Microsoft, etc.)
- At least two machines where you want to measure network performance
- Network connectivity between your machines and the EthrHub service

## Step 1: Create Your Account

1. Visit [www.ethrhub.com](https://www.ethrhub.com)
2. Click **Sign In** in the top right corner
3. Choose your preferred OAuth provider (Google, GitHub, Microsoft, etc.)
4. Authorize EthrHub to access your basic profile information
5. You'll be redirected to your EthrHub dashboard

## Step 2: Download the Ethr Agent

EthrHub uses Microsoft's Ethr tool as the measurement agent. Download the appropriate version for your platform:

### Linux (x64)
```bash
wget https://github.com/microsoft/ethr/releases/latest/download/ethr_linux.zip
unzip ethr_linux.zip
chmod +x ethr
```

### macOS (x64)
```bash
curl -L https://github.com/microsoft/ethr/releases/latest/download/ethr_darwin.zip -o ethr_darwin.zip
unzip ethr_darwin.zip
chmod +x ethr
```

### Windows
Download from: https://github.com/microsoft/ethr/releases/latest/download/ethr_windows.zip

Extract the ZIP file and you're ready to go!

## Step 3: Connect Your First Agent

### Start the Agent
Run the Ethr agent with the hub parameter:

```bash
./ethr -hub https://www.ethrhub.com
```

### Authorize the Agent
When you run the command, you'll see output similar to:

```
======================================================================
AUTHENTICATION REQUIRED
======================================================================

Please visit: https://www.ethrhub.com/device

And enter code: ABCD-1234

Or visit directly: https://www.ethrhub.com/device?code=ABCD-1234

======================================================================
Waiting for authorization...
```

### Complete Authorization
1. Open your browser and visit the URL shown
2. Enter the device code when prompted
3. Confirm that you want to authorize this agent
4. Return to your terminal - the agent will automatically connect!

You should see:
```
Authentication successful!
Agent registered successfully
Agent ID: agent-abc123
Listening for test assignments...
```

## Step 4: Connect Additional Agents

To measure network performance between two points, you need at least two agents:

1. Repeat Step 3 on your second machine
2. Each agent will get a unique ID and appear in your dashboard
3. You can connect as many agents as you need

## Step 5: Create Your First Test

Now that you have agents connected, let's create a test:

1. Go to your dashboard at [www.ethrhub.com](https://www.ethrhub.com)
2. You should see your connected agents in the left sidebar
3. Click the **New Test** button
4. Fill in the test details:
   - **Test Type**: Choose Bandwidth, Latency, Connections, or Packet Loss
   - **Source Agent**: Select the agent that will initiate the test
   - **Destination Agent**: Select the target agent
   - **Duration**: How long the test should run (default: 10 seconds)
   - **Protocol**: TCP or UDP
5. Click **Start Test**

## Step 6: View Results

Once your test starts:

- You'll see it appear in the **All Tests** tab
- The status will change from "Pending" to "Running"
- Real-time metrics will update as the test runs
- When complete, you can view detailed results and historical data

## Next Steps

Now that you're up and running:

- [Learn about different test types](running-tests.md)
- [Explore agent configuration options](agent-installation.md)
- [Check out the API for automation](api-reference.md)
- [Read troubleshooting tips](troubleshooting.md)

## Getting Help

If you run into any issues:

- Check the [FAQ](faq.md)
- Visit our [Discussions forum](https://github.com/ethrhub/ethrhub/discussions)
- [Open an issue](https://github.com/ethrhub/ethrhub/issues) if you found a bug
- Email us at support@ethrhub.com

Welcome to EthrHub! 🚀
