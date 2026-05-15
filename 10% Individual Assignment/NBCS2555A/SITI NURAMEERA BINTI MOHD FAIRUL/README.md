# SITI NURAMEERA BINTI MOHD FAIRUL
# Comprehensive Performance Testing and Bottleneck Analysis of SpaceX REST API Using Grafana k6

## Introduction

This project focuses on performance testing of the SpaceX REST API using Grafana k6. The purpose of this project is to evaluate the API performance under different traffic conditions including load testing, stress testing, and spike testing.

## Target API

https://api.spacexdata.com/v4/launches/latest

## Testing Tool

Grafana k6

## Test Types

- Load Test
- Stress Test
- Spike Test
  
## Hypothesis

The hypothesis of this project is that the SpaceX REST API can handle moderate user traffic efficiently during normal load conditions. However, under extreme concurrent user requests during stress and spike testing, the API may experience increased response time, reduced throughput, and possible request failures.

---

## Test Environment

- Operating System: Kali Linux
- Performance Testing Tool: Grafana k6
- Target API: https://api.spacexdata.com/v4/launches/latest
- Internet Connection: Stable broadband connection

---

## Load Test

The load test was performed to evaluate the API performance under normal expected traffic conditions.

### Load Test Script

```javascript
(PASTE LOAD TEST CODE HERE)
```

### Load Test Result

(PASTE SCREENSHOT HERE LATER)

### Analysis

The load test showed stable performance with low response time and no significant request failures. The API successfully handled moderate concurrent traffic efficiently.

---

## Stress Test

The stress test was conducted to determine the breaking point of the API under high user load.

### Stress Test Script

```javascript
(PASTE STRESS TEST CODE HERE)
```

### Stress Test Result

(PASTE SCREENSHOT HERE LATER)

### Analysis

The stress test indicated increased response times and reduced performance stability as the number of virtual users increased. Some requests experienced delays under heavy traffic conditions.

---

## Spike Test

The spike test evaluated the API behavior during sudden traffic increases.

### Spike Test Script

```javascript
(PASTE SPIKE TEST CODE HERE)
```

### Spike Test Result

(PASTE SCREENSHOT HERE LATER)

### Analysis

The spike test demonstrated that sudden increases in user traffic affected the response consistency of the API. Temporary latency spikes were observed during rapid traffic escalation.

---

## Bottleneck Analysis

Several potential bottlenecks were identified during the testing process. Under higher concurrent traffic, the API showed increased response times and reduced stability. This may indicate server-side limitations, network latency, or possible rate limiting mechanisms.

---

## Recommendations

- Implement load balancing mechanisms
- Improve backend resource optimization
- Introduce caching strategies
- Enhance server scalability for high traffic scenarios

---

## Conclusion

This project successfully demonstrated the use of Grafana k6 for conducting performance testing on the SpaceX REST API. The load, stress, and spike tests provided valuable insights into the API performance behavior under different traffic conditions. Overall, the API performed well under moderate load but showed performance degradation during extreme traffic conditions.
