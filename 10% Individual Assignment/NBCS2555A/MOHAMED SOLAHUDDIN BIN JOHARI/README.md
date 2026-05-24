# PERFORMANCE TESTING REPORT

## Adaptive Performance Monitoring of Web Services Under Dynamic User Patterns

**Course:** ITT440 – Network Programming  
**Tool Used:** Apache JMeter  
**Target API:** https://openlibrary.org/search.json?q=harry+potter

---

# 1. Introduction

Performance testing is important to evaluate the stability, scalability, and responsiveness of a web service when handling multiple users at the same time. In this project, Apache JMeter was used to perform performance testing on the OpenLibrary API.

The selected API is a public API provided by OpenLibrary that allows users to search book information online. This project focuses on analyzing API behavior under different traffic conditions using Load Test, Stress Test, and Spike Test.

The objectives of this project are:

- To evaluate API performance under normal traffic conditions
- To analyze system behavior under heavy user load
- To observe system stability during sudden traffic spikes
- To identify response time and error behavior during testing

---

# 2. Methodology

## 2.1 Tools Used

- Apache JMeter 5.6.3
- Java JDK
- Windows Command Prompt

## 2.2 Target API

The API used for testing:

```bash
https://openlibrary.org/search.json?q=harry+potter
```

## 2.3 Testing Types

| Test Type | Description |
|---|---|
| Load Test | Evaluate system performance under normal increasing traffic |
| Stress Test | Evaluate system behavior under heavy user load |
| Spike Test | Evaluate system stability during sudden traffic spikes |

---

# 3. Test Configuration

## 3.1 Load Test

| Configuration | Value |
|---|---|
| Number of Threads (Users) | 12 |
| Ramp-Up Period | 20 seconds |
| Loop Count | 5 |

### Purpose

The Load Test was conducted to analyze API performance under normal user traffic conditions.

---

## 3.2 Stress Test

| Configuration | Value |
|---|---|
| Number of Threads (Users) | 50 |
| Ramp-Up Period | 20 seconds |
| Loop Count | 5 |

### Purpose

The Stress Test was conducted to determine how the system behaves when handling heavy user traffic.

---

## 3.3 Spike Test

| Configuration | Value |
|---|---|
| Number of Threads (Users) | 80 |
| Ramp-Up Period | 1 second |
| Loop Count | 3 |

### Purpose

The Spike Test was conducted to analyze system behavior when sudden high traffic occurs.

---

# 4. Results and Analysis

## 4.1 Load Test Result

### Summary

| Metric | Result |
|---|---|
| APDEX Score | 0.275 |
| Pass Rate | 100% |
| Fail Rate | 0% |
| Average Response Time | 2226.67 ms |
| Maximum Response Time | 6404 ms |
| Throughput | 3.13 transactions/s |

### Analysis

For the Load Test, the system managed to process all requests successfully without any errors during the testing session.

The average response time was around 2226 milliseconds. This shows that the API was still stable under normal traffic, although a few requests took slightly longer to respond.

The APDEX score recorded was 0.275, which is considered Very Poor because the response time was still quite high.

From the Response Time Percentiles graph, most requests were completed within a few seconds, but some requests took longer to respond.

The Time Vs Threads graph shows that response time increased when the number of users increased, indicating slower system performance under higher traffic.

---

## 4.2 Stress Test Result

### Summary

| Metric | Result |
|---|---|
| APDEX Score | 0.054 |
| Pass Rate | 46.4% |
| Fail Rate | 53.6% |
| Average Response Time | 2099.11 ms |
| Maximum Response Time | 8760 ms |
| Throughput | 6.80 transactions/s |
| Error Type | 429 Too Many Requests |

### Analysis

During the Stress Test, the system performance started to decrease when more users accessed the API at the same time.

The APDEX score recorded was 0.054, which is categorized as Very Poor and indicates low user satisfaction during the test.

Only 46.4 percent of the requests were successful, while 53.6 percent failed because of the “429 Too Many Requests” error.

The average response time recorded was around 2099 milliseconds, and some requests took up to 8760 milliseconds to respond.

From the Response Time Percentiles graph, the response time increased significantly under heavier load conditions.

The Time Vs Threads graph shows that the response time became higher as the number of active users increased, indicating unstable system performance during the Stress Test.

---

## 4.3 Spike Test Result

### Summary

| Metric | Result |
|---|---|
| APDEX Score | 0.000 |
| Pass Rate | 22.08% |
| Fail Rate | 77.92% |
| Average Response Time | 1547.35 ms |
| Maximum Response Time | 8796 ms |
| Throughput | 15.19 transactions/s |
| Error Type | 429 Too Many Requests |

### Analysis

For the Spike Test, the system became unstable when sudden high traffic was sent to the server.

The APDEX score recorded was 0.000, which is categorized as Very Poor and indicates very low user satisfaction during the test.

Besides that, only 22.08 percent of requests passed, while 77.92 percent failed due to the “429 Too Many Requests” error.

The average response time recorded was around 1547 milliseconds, and some requests took up to 8796 milliseconds to respond.

From the Response Time Percentiles graph, the response time increased sharply during sudden traffic spikes.

The Time Vs Threads graph also shows unstable response times when the number of users suddenly increased, which means the system struggled to handle sudden traffic spikes efficiently.

---

# 5. Discussion

The performance testing results show that the OpenLibrary API was able to handle normal traffic during the Load Test without recording any failed requests. However, the response time was still considered high, resulting in a low APDEX score.

During the Stress Test and Spike Test, the system performance became unstable when handling heavier traffic conditions. A large number of requests failed due to the “429 Too Many Requests” error, which indicates that the server implemented rate limiting when too many requests were received.

The graphs also show that the response time increased as the number of users increased. This indicates that the API performance decreases under higher traffic conditions.

Overall, the OpenLibrary API works well under normal traffic conditions, but its performance becomes unstable when handling heavy traffic and sudden spikes.

---

# 6. Conclusion

In conclusion, Apache JMeter successfully analyzed the performance of the OpenLibrary API under different traffic conditions.

The Load Test showed that the system remained stable under normal traffic with no failed requests. However, the Stress Test and Spike Test revealed that the API performance became unstable under heavier traffic conditions.

The high number of “429 Too Many Requests” errors indicates that the server has traffic limitations and cannot efficiently handle sudden or excessive requests.

Overall, this project successfully demonstrated how performance testing can be used to analyze system stability, response time, scalability, and user satisfaction under dynamic user patterns.

---

#Demo Video

https://www.youtube.com/watch?v=xofH9bl939U

# 7. References

Apache JMeter. (2024). *Apache JMeter user manual*. Apache Software Foundation. https://jmeter.apache.org/

Kelas Sir. (2024). *Apache JMeter tutorial videos* [YouTube channel]. YouTube. https://www.youtube.com/@kelassir5655

OpenLibrary. (2024). *OpenLibrary API documentation*. https://openlibrary.org/developers/api

Oracle. (2024). *Java documentation*. Oracle Corporation. https://www.oracle.com/java/

OutSystems. (2024). *The Apdex performance score*. https://success.outsystems.com/documentation/11/monitoring_and_troubleshooting_apps/the_apdex_performance_score/
