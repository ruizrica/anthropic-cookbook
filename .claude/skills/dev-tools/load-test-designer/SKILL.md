---
name: load-test-designer
description: Design and execute load tests using k6, Artillery, and JMeter to validate performance under stress
---

# Load Test Designer

## k6 Load Testing

```javascript
// load-test.js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '1m', target: 50 },   // Ramp up to 50 users
    { duration: '3m', target: 50 },   // Stay at 50 users
    { duration: '1m', target: 100 },  // Ramp up to 100 users
    { duration: '3m', target: 100 },  // Stay at 100
    { duration: '1m', target: 0 },    // Ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<500'],  // 95% of requests < 500ms
    http_req_failed: ['rate<0.01'],    // Error rate < 1%
  },
};

export default function () {
  const res = http.get('https://api.example.com/users');

  check(res, {
    'status is 200': (r) => r.status === 200,
    'response time < 500ms': (r) => r.timings.duration < 500,
  });

  sleep(1);
}
```

Run:
```bash
k6 run load-test.js
```

## Artillery

```yaml
# artillery-config.yml
config:
  target: 'https://api.example.com'
  phases:
    - duration: 60
      arrivalRate: 10  # 10 requests/sec
    - duration: 120
      arrivalRate: 50  # Ramp to 50 req/sec
  defaults:
    headers:
      Authorization: 'Bearer {{token}}'

scenarios:
  - name: 'Get users'
    flow:
      - get:
          url: '/api/users'
      - think: 2
      - get:
          url: '/api/users/{{$randomString()}}'
```

Run:
```bash
artillery run artillery-config.yml
```

## Performance Metrics

| Metric | Description | Target |
|--------|-------------|--------|
| Response Time (p50) | Median response time | < 200ms |
| Response Time (p95) | 95th percentile | < 500ms |
| Response Time (p99) | 99th percentile | < 1000ms |
| Throughput | Requests per second | > 1000 RPS |
| Error Rate | Failed requests | < 1% |
| Concurrent Users | Active users | Scale to 10k+ |

## Load Test Scenarios

### 1. Spike Test
```javascript
export const options = {
  stages: [
    { duration: '10s', target: 10 },
    { duration: '10s', target: 1000 },  // Sudden spike
    { duration: '10s', target: 10 },
  ],
};
```

### 2. Stress Test
```javascript
export const options = {
  stages: [
    { duration: '2m', target: 100 },
    { duration: '5m', target: 100 },
    { duration: '2m', target: 200 },
    { duration: '5m', target: 200 },
    { duration: '2m', target: 300 },  // Keep increasing
  ],
};
```

### 3. Soak Test (Endurance)
```javascript
export const options = {
  stages: [
    { duration: '5m', target: 100 },
    { duration: '24h', target: 100 },  // Long duration
    { duration: '5m', target: 0 },
  ],
};
```
