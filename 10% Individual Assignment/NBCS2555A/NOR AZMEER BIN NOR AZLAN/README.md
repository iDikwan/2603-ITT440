# Evaluating High-Concurrency Throughput and Latency of RESTful Services using Autocannon

## Student Information
* **Name:** Nor Azmeer Bin Nor Azlan 
* **Student ID:** 2024389117 
* **Group:** NBCS2555A 
* **Course:** ITT440 - Network Programming 

## 1. Introduction
In modern application development, APIs play a crucial role in enabling communication between systems.This study focuses on evaluating the performance of a RESTful service using the **Platzi Fake Store API**. 

The main objectives are:
* To measure throughput performance under baseline conditions.
* To identify server breaking points under extreme traffic surges.
* To analyze error handling and system stability during stress scenarios.

## 2. Methodology
### 2.1 Tools Used
* **Autocannon:** Node.js-based HTTP benchmarking tool.
* **Environment:** Node.js v24.15.0 on Windows.

### 2.2 Target API
* **Endpoint:** `https://api.escuelajs.co/api/v1/products` 

## 3. Test Results & Analysis

### 3.1 Load Test (Baseline)
Analyzes performance as traffic remains at a normal operational level.
* **Command:** `autocannon -c 20 -d 30 [URL]` 
* **Result:** 70.87 req/s with 0% error rate.

![Load Test Result](./Load%20Test.JPG)

---

### 3.2 Stress Test (Peak Traffic)
Increases concurrency to 100 users to test server endurance.
* **Command:** `autocannon -c 100 -d 30 [URL]` 
* **Result:** 400.17 req/s; system remained stable.

![Stress Test Result](./Stress%20Test.JPG)

---

### 3.3 Flood/Spike Test (Extreme Surge)
A sudden jump to 500 users with pipelining to detect the system's breaking point.
* **Command:** `autocannon -c 500 -p 10 -d 20 [URL]` 
* **Result:** Latency spiked to 1.7s (Avg) and 12.4s (Max) with **5,000 timeout errors**.

![Flood Test Result](./Flood%20and%20Spike%20Test.JPG)

## 4. Discussion
The performance evaluation reveals that the system is highly stable under normal and peak traffic (up to 100 connections).However, the Flood Test identified a clear breaking point where the API could no longer provide reliable data delivery due to resource limits or rate-limiting.

## 5. Conclusion
This study successfully evaluated the performance limits of the target API. While efficient for standard use, the system degrades significantly under extreme loads (15% failure rate). To enhance stability, server-side optimizations such as load balancing and stricter rate-limiting are recommended.
