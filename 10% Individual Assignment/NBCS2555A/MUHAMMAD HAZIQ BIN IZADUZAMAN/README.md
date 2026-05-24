# Performance Testing of Cat Facts API Using Grafana K6

## ITT440: Individual Assignment Report

| Details | Information |
|---|---|
| Course | ITT440 - Network Programming |
| Group | NBCS2555A |
| Name | Muhammad Haziq Bin Izaduzaman |
| Student ID | 2024750117 |
| Lecturer | Sir Shahadan Bin Saad |

---

# Introduction

Performance testing is an important process used to evaluate how a system performs under different levels of user load, ensuring that it remains stable, responsive, and reliable in real-world conditions.In this assignment, k6 is used as a performance testing tool to simulate different scenarios including load testing, stress testing, and spike testing on a sample API.These tests help to measure system behavior under normal usage, heavy traffic, and sudden increases in users, allowing us to identify performance issues, response time delays, and system limitations.

## Main Objectives

- To evaluate API performance under concurrent users
- To measure response time and throughput
- To identify system bottlenecks under high load

---

# Methodology

## Tools Used

This project uses Grafana k6 as the performance testing tool.

k6 is an open-source load testing tool used to simulate virtual users and measure system performance such as response time, stability, and reliability under different traffic conditions.

---

# Target API

The API used in this project is:

```bash
https://catfact.ninja/fact
```

---

# Testing Approach

This project implements three types of performance testing using k6:

| Test Type | Description |
|---|---|
| Load Test | Simulates 50 virtual users for 1 minute to evaluate normal system performance under expected traffic conditions |
| Stress Test | Simulates 300 virtual users for 1 minute to test system behavior under high load and identify performance limits |
| Spike Test | Simulates a sudden increase in traffic from 10 to 1000 users to evaluate system stability under unexpected traffic spikes |

---

# Test Design and Coding

# LOAD TEST

- Start = 0 User
- Peak = 50 Users
- Duration for 1 minute
- Simulated 50 virtual users for 1 minute to test normal API performance.

## Load Test Script

```javascript
import http from 'k6/http';
import { sleep } from 'k6';

export const options = {
  stages: [
    { duration: '10s', target: 0 },
    { duration: '40s', target: 50 },
    { duration: '10s', target: 0 },
  ],
};

export default function () {
  http.get('https://catfact.ninja/fact');
  sleep(1);
}
```

---

# STRESS TEST

- Start = 0 User
- Peak = 300 Users
- Duration for 1 minute
- Increase 300 virtual users for 1 minute to test system performance under heavy load.

## Stress Test Script

```javascript
import http from 'k6/http';
import { sleep } from 'k6';

export const options = {
  stages: [
    { duration: '10s', target: 0 },
    { duration: '40s', target: 300 },
    { duration: '10s', target: 0 },
  ],
};

export default function () {
  http.get('https://catfact.ninja/fact');
  sleep(1);
}
```

---

# SPIKE TEST

- Sudden increase from 10 to 1000 virtual users

## Spike Test Script

```javascript
import http from 'k6/http';
import { sleep } from 'k6';

export const options = {
  stages: [
    { duration: '10s', target: 10 },
    { duration: '10s', target: 1000 },
    { duration: '20s', target: 1000 },
    { duration: '10s', target: 0 },
  ],
};

export default function () {
  http.get('https://catfact.ninja/fact');
  sleep(1);
}
```

---

# Results

# LOAD TEST

| Metric | Result |
|---|---|
| Average Response Time | 481.31 ms |
| Median Response Time | 365.98 ms |
| Min Response Time | 268.12 ms |
| Max Response Time | 2.56 s |
| 95th Percentile | 929.02 ms |
| 90th Percentile | 797.58 ms |
| Total Requests | 858 |
| Request Rate | 14.129769/s |
| Error Rate | 88.34% (758 failed) |

## Load Test Result Screenshot


<img width="958" height="678" alt="image" src="https://github.com/user-attachments/assets/f0e4c6d6-9ba2-42ae-a87d-5f0fc6d9734e" />




---

# STRESS TEST

| Metric | Result |
|---|---|
| Average Response Time | 505.07 ms |
| Median Response Time | 389.35 ms |
| Min Response Time | 257.7 ms |
| Max Response Time | 4.85 s |
| 95th Percentile | 1.08 s |
| 90th Percentile | 852.66 ms |
| Total Requests | 5097 |
| Request Rate | 83.025828/s |
| Error Rate | 98.03% (4997 failed) |

## Stress Test Result Screenshot

<img width="948" height="718" alt="image" src="https://github.com/user-attachments/assets/fa3a7918-13d9-4488-af5a-ad2611e65805" />


---

# SPIKE TEST

| Metric | Result |
|---|---|
| Average Response Time | 2.38 s |
| Median Response Time | 2.13 s |
| Min Response Time | 258.81 ms |
| Max Response Time | 9.33 s |
| 95th Percentile | 5.99 s |
| 90th Percentile | 4.59 s |
| Total Requests | 9785 |
| Request Rate | 178.81593/s |
| Error Rate | 98.97% (9685 failed) |

## Spike Test Result Screenshot

<img width="964" height="718" alt="image" src="https://github.com/user-attachments/assets/dec264b1-d052-43be-87b7-d2a564a49c31" />



---

# Conclusion

Based on the performance testing results conducted on the Cat Facts API (https://catfact.ninja/fact) using Grafana k6, the system shows mixed performance under different load conditions. During the Load Test, the API achieved an average response time of 481.31 ms; however, it recorded a high error rate of 88.34%, indicating instability even under normal traffic. In the Stress Test, the system handled increased traffic with a higher request rate of 83.03 requests per second, but the error rate further increased to 98.03%, showing significant performance degradation under heavy load. The Spike Test revealed the weakest performance, where sudden traffic increase caused the average response time to rise to 2.38 seconds with a maximum of 9.33 seconds and an error rate of 98.97%, indicating that the system struggles to maintain stability during traffic spikes. Overall, the results show that while the API can respond to requests, it is not stable under high or sudden load conditions and experiences a high rate of failures as traffic increases.
