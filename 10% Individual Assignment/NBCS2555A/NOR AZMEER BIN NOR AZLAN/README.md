# Benchmarking High-Throughput Performance of Stateless User-Management Services

## Student Information
* **Name:** Nor Azmeer Bin Nor Azlan [cite: 6]
* [cite_start]**Student ID:** 2024389117 [cite: 6]
* [cite_start]**Group:** NBCS2555A [cite: 6]
* [cite_start]**Course:** ITT440 - Network Programming [cite: 2]

## Introduction
[cite_start]This project evaluates the performance of a RESTful service using the **Platzi Fake Store API**[cite: 10]. [cite_start]The study measures throughput, identifies server breaking points, and analyzes error handling under stress [cite: 11-14].

## Methodology
### Tools Used
* [cite_start]**Autocannon:** Node.js-based HTTP benchmarking tool[cite: 18].
* [cite_start]**Environment:** Node.js v24.15.0 on Windows[cite: 22].

### Target API
* [cite_start]**Endpoint:** `https://api.escuelajs.co/api/v1/products` [cite: 21]

## Test Scenarios & Results

| Test Type | Connections | Command | Avg Latency | Avg Req/Sec | Success Rate |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Load Test** | 20 | `autocannon -c 20 -d 30` | 283.25 ms | 70.87 | [cite_start]100% [cite: 39-42] |
| **Stress Test** | 100 | `autocannon -c 100 -d 30` | 249.64 ms | 400.17 | [cite_start]100% [cite: 44-47] |
| **Flood Test** | 500 (p10) | `autocannon -c 500 -p 10 -d 20` | 1710.85 ms | 1,141.25 | [cite_start]85% [cite: 49-52, 64] |

## Discussion
* [cite_start]**Stability:** The system remained stable up to 100 concurrent connections[cite: 56].
* [cite_start]**Breaking Point:** At 500 connections with pipelining, the server exceeded resource limits, resulting in **5,000 timeout errors**[cite: 59].
* [cite_start]**Latency:** Average latency spiked from ~280ms to 1.7 seconds during the flood test[cite: 55, 58].

## Conclusion
[cite_start]The API is suitable for standard use but requires horizontal scaling or stricter rate-limiting to handle enterprise-level spikes[cite: 65, 66].
