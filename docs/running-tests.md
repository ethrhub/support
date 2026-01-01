# Running Tests

Learn how to create and run network performance tests with EthrHub.

## Test Types

EthrHub supports multiple test types to measure different aspects of network performance:

### Bandwidth Test
Measures maximum throughput between two agents.

**Use cases:**
- Validate network capacity
- Identify bandwidth bottlenecks
- Compare different network paths

**Metrics:**
- Throughput (Mbps/Gbps)
- Packets per second
- Average bandwidth over duration

### Latency Test
Measures round-trip time (RTT) between agents.

**Use cases:**
- Measure network responsiveness
- Identify high-latency paths
- Monitor SLA compliance

**Metrics:**
- Minimum latency
- Average latency
- Maximum latency
- Standard deviation

### Connections Test
Measures maximum concurrent connections.

**Use cases:**
- Test load balancer capacity
- Validate connection pooling
- Stress test services

**Metrics:**
- Successful connections
- Failed connections
- Connection rate

### Packet Loss Test
Measures packet drop rate.

**Use cases:**
- Identify unreliable links
- Diagnose network congestion
- Monitor quality of service

**Metrics:**
- Packets sent
- Packets received
- Loss percentage

## Creating a Test

### From the Dashboard

1. Click **New Test** button
2. Select **Test Type** (Bandwidth, Latency, etc.)
3. Choose **Source Agent** - the agent that will initiate the test
4. Choose **Destination Agent** - the target agent
5. Configure test parameters:
   - **Duration**: How long to run the test (1-300 seconds)
   - **Protocol**: TCP or UDP
   - **Threads**: Number of parallel connections (1-64)
   - **Buffer Size**: Packet size for the test
6. Click **Start Test**

### Test Parameters Explained

#### Duration
- **Short tests (1-10s)**: Quick spot checks
- **Medium tests (10-60s)**: Standard measurements
- **Long tests (60-300s)**: Comprehensive analysis

#### Protocol
- **TCP**: Reliable, connection-oriented (default)
- **UDP**: Faster, connectionless (for latency tests)

#### Threads
- More threads = higher aggregate throughput
- Use 1 thread for baseline measurements
- Use multiple threads to max out bandwidth

#### Buffer Size
- **Small (1KB-64KB)**: For latency-sensitive tests
- **Large (128KB-1MB)**: For maximum throughput

## Test Status

Tests can have the following statuses:

- **Pending**: Waiting to start
- **Running**: Currently executing
- **Completed**: Finished successfully
- **Failed**: Error occurred
- **Cancelled**: Manually stopped

## Viewing Results

### Real-time Monitoring

While a test is running:
- View live throughput graph
- See current metrics
- Monitor agent status

### Test History

After completion:
- Click on any test card to view details
- See detailed metrics and graphs
- Compare with previous tests
- Export data for analysis

## Test Best Practices

### 1. Establish Baselines
Run tests during different times to understand normal performance:
```
- Off-peak hours (low load)
- Peak hours (high load)
- Different days of the week
```

### 2. Use Consistent Parameters
For comparisons, keep test parameters constant:
- Same duration
- Same protocol
- Same thread count

### 3. Consider Network Load
- Avoid running tests during critical operations
- Be aware that tests consume bandwidth
- Coordinate with network administrators

### 4. Test Both Directions
Network performance can be asymmetric:
- Run tests A→B
- Run tests B→A
- Compare results

### 5. Multiple Iterations
Run tests multiple times to account for variance:
- Take average of 3-5 runs
- Discard outliers
- Look for consistency

## Common Test Scenarios

### Scenario 1: New Link Validation
**Goal**: Verify a new network link meets specifications

```
Test Type: Bandwidth
Duration: 30 seconds
Protocol: TCP
Threads: 8
```

Run bidirectional tests and verify throughput meets SLA.

### Scenario 2: Latency Monitoring
**Goal**: Continuous latency monitoring for SLA compliance

```
Test Type: Latency
Duration: 10 seconds
Protocol: UDP
Threads: 1
```

Schedule regular tests and alert on threshold violations.

### Scenario 3: Troubleshooting Slow Performance
**Goal**: Diagnose why applications are slow

```
1. Run latency test (identify if it's RTT issue)
2. Run packet loss test (check for drops)
3. Run bandwidth test (verify available capacity)
```

### Scenario 4: Load Testing
**Goal**: Validate system can handle expected load

```
Test Type: Connections
Duration: 60 seconds
Protocol: TCP
Threads: 32
```

Gradually increase connection count to find limits.

## Interpreting Results

### Bandwidth Tests

**Good Results:**
- Throughput close to link capacity
- Consistent throughout test
- Low variance between runs

**Problem Indicators:**
- Much lower than expected throughput
- Highly variable throughput
- Decreasing throughput over time

### Latency Tests

**Good Results:**
- Consistent RTT values
- Low standard deviation
- Minimal packet loss

**Problem Indicators:**
- High average latency (>100ms for local)
- High variance (jitter)
- Increasing latency over time

### Connection Tests

**Good Results:**
- All connections successful
- Connection rate meets requirements
- No timeouts

**Problem Indicators:**
- High failure rate
- Slow connection establishment
- Timeouts or resets

### Packet Loss Tests

**Good Results:**
- 0% packet loss
- Consistent delivery
- No retransmissions

**Problem Indicators:**
- Any packet loss (>0.1% is concerning)
- Increasing loss over time
- Patterns suggesting congestion

## Scheduling Tests

*Coming soon: Automated test scheduling*

Future releases will include:
- Recurring test schedules
- Automated alerting
- Trend analysis
- Performance reports

## Test Limits

### Basic Plan
- Up to 5 agents
- 50 tests per day
- 30 days of history

### Premium Plan
- Up to 100 agents
- 1,000 tests per day
- Unlimited history
- Priority test queue

## Troubleshooting

### Test Fails to Start
- Verify both agents are connected
- Check agents can reach each other
- Ensure no firewall blocking test ports
- Confirm agents aren't busy with other tests

### Inconsistent Results
- Check for network congestion
- Verify no background traffic during test
- Ensure agents have sufficient resources
- Try increasing test duration

### Low Performance
- Check agent CPU/memory usage
- Verify network path is correct
- Test with different parameters
- Compare against baseline tests

## Next Steps

- [Understand agent installation](agent-installation.md)
- [Check troubleshooting guide](troubleshooting.md)
- [Ask questions in discussions](https://github.com/ethrhub/support/discussions)
