# API Reference

EthrHub provides a REST API for programmatic access to platform features. API access is available on Premium plans.

## Getting Started

### Authentication

All API requests require authentication using a Bearer token.

**Get Your API Token:**
1. Log in to your EthrHub dashboard
2. Go to Settings
3. Navigate to API section
4. Generate or copy your API token

**Using the Token:**
```bash
curl -H "Authorization: Bearer YOUR_API_TOKEN" \
     https://www.ethrhub.com/api/endpoint
```

### Base URL
```
https://www.ethrhub.com/api
```

### Response Format
All responses are in JSON format.

**Success Response:**
```json
{
  "success": true,
  "data": { ... }
}
```

**Error Response:**
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message"
  }
}
```

### Rate Limits

- **Basic Plan:** Not available
- **Premium Plan:** 1000 requests/hour

Rate limit headers are included in responses:
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1640000000
```

## Agents

### List Agents

Get all agents associated with your account.

**Endpoint:** `GET /api/agent`

**Example:**
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     https://www.ethrhub.com/api/agent
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "agent-abc123",
      "name": "Production Server 1",
      "status": "Connected",
      "lastSeen": "2025-12-20T10:30:00Z",
      "ipAddress": "192.168.1.100",
      "version": "1.0.0"
    }
  ]
}
```

### Get Agent Details

Get detailed information about a specific agent.

**Endpoint:** `GET /api/agent/{agentId}`

**Parameters:**
- `agentId` (path) - Agent ID

**Example:**
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     https://www.ethrhub.com/api/agent/agent-abc123
```

### Update Agent

Update agent properties like name or description.

**Endpoint:** `PUT /api/agent/{agentId}`

**Body:**
```json
{
  "name": "Updated Agent Name",
  "description": "Optional description"
}
```

**Example:**
```bash
curl -X PUT \
     -H "Authorization: Bearer YOUR_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"name":"New Name"}' \
     https://www.ethrhub.com/api/agent/agent-abc123
```

### Delete Agent

Remove an agent from your account.

**Endpoint:** `DELETE /api/agent/{agentId}`

**Example:**
```bash
curl -X DELETE \
     -H "Authorization: Bearer YOUR_TOKEN" \
     https://www.ethrhub.com/api/agent/agent-abc123
```

## Tests

### Create Test

Start a new network performance test.

**Endpoint:** `POST /api/test`

**Body:**
```json
{
  "type": "bandwidth|latency|connections|packetloss",
  "sourceAgentId": "agent-abc123",
  "destAgentId": "agent-def456",
  "duration": 30,
  "protocol": "tcp|udp",
  "threads": 1,
  "bufferSize": 65536
}
```

**Parameters:**
- `type` (required) - Test type
- `sourceAgentId` (required) - Source agent ID
- `destAgentId` (required) - Destination agent ID
- `duration` (optional) - Test duration in seconds (1-300, default: 10)
- `protocol` (optional) - Protocol to use (default: tcp)
- `threads` (optional) - Number of parallel connections (1-64, default: 1)
- `bufferSize` (optional) - Buffer size in bytes (default: 65536)

**Example:**
```bash
curl -X POST \
     -H "Authorization: Bearer YOUR_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
       "type": "bandwidth",
       "sourceAgentId": "agent-abc123",
       "destAgentId": "agent-def456",
       "duration": 30,
       "protocol": "tcp"
     }' \
     https://www.ethrhub.com/api/test
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "test-xyz789",
    "status": "Pending",
    "createdAt": "2025-12-20T10:35:00Z"
  }
}
```

### List Tests

Get all tests for your account.

**Endpoint:** `GET /api/test`

**Query Parameters:**
- `status` (optional) - Filter by status (pending, running, completed, failed)
- `limit` (optional) - Number of results (1-100, default: 50)
- `offset` (optional) - Offset for pagination (default: 0)
- `startDate` (optional) - Filter tests after this date (ISO 8601)
- `endDate` (optional) - Filter tests before this date (ISO 8601)

**Example:**
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     "https://www.ethrhub.com/api/test?status=completed&limit=10"
```

**Response:**
```json
{
  "success": true,
  "data": {
    "tests": [
      {
        "id": "test-xyz789",
        "type": "bandwidth",
        "status": "Completed",
        "sourceAgentId": "agent-abc123",
        "destAgentId": "agent-def456",
        "startedAt": "2025-12-20T10:35:00Z",
        "completedAt": "2025-12-20T10:35:30Z",
        "results": {
          "throughput": 950.5,
          "unit": "Mbps"
        }
      }
    ],
    "total": 150,
    "limit": 10,
    "offset": 0
  }
}
```

### Get Test Details

Get detailed results for a specific test.

**Endpoint:** `GET /api/test/{testId}`

**Example:**
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     https://www.ethrhub.com/api/test/test-xyz789
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "test-xyz789",
    "type": "bandwidth",
    "status": "Completed",
    "sourceAgent": {
      "id": "agent-abc123",
      "name": "Server 1"
    },
    "destAgent": {
      "id": "agent-def456",
      "name": "Server 2"
    },
    "parameters": {
      "duration": 30,
      "protocol": "tcp",
      "threads": 1
    },
    "startedAt": "2025-12-20T10:35:00Z",
    "completedAt": "2025-12-20T10:35:30Z",
    "results": {
      "throughput": 950.5,
      "unit": "Mbps",
      "packets": 125000,
      "retransmissions": 12
    }
  }
}
```

### Cancel Test

Stop a running test.

**Endpoint:** `DELETE /api/test/{testId}`

**Example:**
```bash
curl -X DELETE \
     -H "Authorization: Bearer YOUR_TOKEN" \
     https://www.ethrhub.com/api/test/test-xyz789
```

## User

### Get User Profile

Get current user information.

**Endpoint:** `GET /api/user/profile`

**Example:**
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     https://www.ethrhub.com/api/user/profile
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "user-123",
    "email": "user@example.com",
    "name": "John Doe",
    "plan": "Premium",
    "createdAt": "2025-01-15T10:00:00Z"
  }
}
```

## Error Codes

| Code | Description |
|------|-------------|
| `UNAUTHORIZED` | Invalid or missing API token |
| `FORBIDDEN` | Access denied to resource |
| `NOT_FOUND` | Resource not found |
| `INVALID_REQUEST` | Request validation failed |
| `RATE_LIMIT_EXCEEDED` | Too many requests |
| `AGENT_OFFLINE` | Agent is not connected |
| `AGENT_BUSY` | Agent is running another test |
| `TEST_FAILED` | Test execution failed |
| `INSUFFICIENT_PERMISSIONS` | Action requires higher plan |

## SDKs & Examples

### Python Example

```python
import requests

API_TOKEN = "your_api_token"
BASE_URL = "https://www.ethrhub.com/api"

headers = {
    "Authorization": f"Bearer {API_TOKEN}",
    "Content-Type": "application/json"
}

# List agents
response = requests.get(f"{BASE_URL}/agent", headers=headers)
agents = response.json()["data"]

# Create test
test_data = {
    "type": "bandwidth",
    "sourceAgentId": agents[0]["id"],
    "destAgentId": agents[1]["id"],
    "duration": 30
}
response = requests.post(f"{BASE_URL}/test", json=test_data, headers=headers)
test = response.json()["data"]

print(f"Test created: {test['id']}")
```

### Node.js Example

```javascript
const axios = require('axios');

const API_TOKEN = 'your_api_token';
const BASE_URL = 'https://www.ethrhub.com/api';

const headers = {
  'Authorization': `Bearer ${API_TOKEN}`,
  'Content-Type': 'application/json'
};

// List agents
async function listAgents() {
  const response = await axios.get(`${BASE_URL}/agent`, { headers });
  return response.data.data;
}

// Create test
async function createTest(sourceId, destId) {
  const testData = {
    type: 'bandwidth',
    sourceAgentId: sourceId,
    destAgentId: destId,
    duration: 30
  };
  const response = await axios.post(`${BASE_URL}/test`, testData, { headers });
  return response.data.data;
}
```

### cURL Examples

**List all agents:**
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     https://www.ethrhub.com/api/agent
```

**Create bandwidth test:**
```bash
curl -X POST \
     -H "Authorization: Bearer YOUR_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
       "type": "bandwidth",
       "sourceAgentId": "agent-abc123",
       "destAgentId": "agent-def456",
       "duration": 30
     }' \
     https://www.ethrhub.com/api/test
```

**Get test results:**
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     https://www.ethrhub.com/api/test/test-xyz789
```

## Best Practices

1. **Store tokens securely** - Never commit API tokens to version control
2. **Handle rate limits** - Implement exponential backoff
3. **Poll responsibly** - Don't poll test status more than once per second
4. **Use webhooks** - Coming soon for real-time updates
5. **Handle errors gracefully** - Always check response status
6. **Cache agent lists** - Don't fetch agent list for every test

## Support

For API support:
- Premium customers: support@ethrhub.com (4-8 hour response)
- Community: https://github.com/ethrhub/hub/discussions

## Changelog

### v1.0.0 (Current)
- Initial API release
- Agent management endpoints
- Test creation and monitoring
- User profile access

*Future releases will include webhooks, scheduled tests, and advanced analytics APIs.*
