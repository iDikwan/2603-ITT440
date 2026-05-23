# Comprehensive Performance Testing and Bottleneck Analysis of the JSONPlaceholder REST API Using Grafana k6

> **ITT440 Individual Assignment** | Solo Submission
> **Target API:** `https://jsonplaceholder.typicode.com/posts`
> **Primary Tool:** Grafana k6 v2.0.0 (JavaScript test scripts)
> **Test Types:** Load Test · Stress Test · Spike Test

---

## Table of Contents
1. [How It Works](#how-it-works)
2. [Problem Statement](#problem-statement)
3. [Hypothesis](#hypothesis)
4. [Why I Chose k6 (Tool Justification)](#why-i-chose-k6-tool-justification)
5. [System Requirements](#system-requirements)
6. [Installation](#installation)
7. [How to Run](#how-to-run)
8. [Project Structure](#project-structure)
9. [Test Types & Methodology](#test-types--methodology)
10. [Key Metrics Explained](#key-metrics-explained)
11. [Results & Analysis](#results--analysis)
12. [Identified Bottlenecks](#identified-bottlenecks)
13. [Recommendations](#recommendations)
14. [Conclusion](#conclusion)
15. [Demo Video](#demo-video)

---

## How It Works

```
Maelicious88's PC (client)  ──────────────────────────►  jsonplaceholder.typicode.com
                            sends HTTP GET / POST             (public REST API server)
                            requests via k6                            │
Maelicious88's PC (client)  ◄──────────────────────────  returns JSON response
                            k6 measures response time,
                            throughput and error rate
```

- **Maelicious88's PC** is the **client** — Grafana k6 runs here and simulates many virtual users (VUs).
- `jsonplaceholder.typicode.com/posts` is the **server** — a free public REST API that returns JSON.
- The **internet** is the bridge between them, so latency varies slightly on every run depending on real network conditions at that moment.
- k6 records how long each request takes, how many succeed or fail, and how many requests per second the system sustains.

---

## Problem Statement

Modern web APIs must cope with very different traffic patterns: steady everyday use, sustained heavy demand, and sudden unexpected bursts. Knowing **where** and **when** performance degrades is essential for building reliable systems.

This project investigates the performance characteristics of the public REST API endpoint `https://jsonplaceholder.typicode.com/posts` under three distinct traffic profiles, using Grafana k6:

| Test Type | Goal | Load Profile |
|-----------|------|--------------|
| **Load Test** | Confirm stable performance under normal expected traffic | Ramp to 20 VUs, hold 1 min, ramp down |
| **Stress Test** | Find the breaking point beyond normal capacity | Staircase: 50 → 100 → 200 → 300 VUs |
| **Spike Test** | Test survival and recovery from a sudden burst | Jump 10 → 300 VUs in 10s, hold, then drop |

The aim is to interpret the resulting metrics — response time, throughput, and error rate — to identify performance bottlenecks and recommend improvements.

---

## Hypothesis

Before running the tests, I predicted the following:

1. Under **normal load** (20 VUs), the API would respond quickly and reliably — I expected 95% of requests under ~500 ms and a near-zero error rate.
2. Under the **stress test**, response times would climb steadily as virtual users increased, and errors would begin to appear beyond roughly 100–200 VUs, because JSONPlaceholder is a shared free service that may rate-limit heavy traffic.
3. Under the **spike test**, the sudden burst would cause a sharp temporary jump in response time and possibly some failed requests, but the API should recover once the spike passed.

As the [Results & Analysis](#results--analysis) section shows, **my real results were surprising and partly contradicted these predictions** — which is itself an interesting finding.

---

## Why I Chose k6 (Tool Justification)

I selected **Grafana k6** for the following reasons:

- **Developer-friendly scripting.** Tests are written in plain JavaScript, which is readable and easy to version-control on GitHub — unlike GUI-only tools.
- **Built for high load with low overhead.** k6's engine is written in Go, so a single machine can simulate hundreds of virtual users efficiently. This made it possible to run my 300-VU stress and spike tests on an ordinary laptop.
- **Clear, built-in metrics.** k6 reports `http_req_duration`, `http_req_failed`, throughput, and percentiles out of the box, and supports custom metrics and pass/fail **thresholds**.
- **Flexible load profiles.** The `stages` option lets me precisely model ramp-up, sustained load, sudden spikes, and recovery — exactly the three traffic patterns this assignment requires.
- **Industry adoption.** k6 (by Grafana Labs) is widely used in real-world performance testing and integrates with CI/CD pipelines, so the skills transfer directly to professional work.

---

## System Requirements

| Component | Minimum |
|-----------|---------|
| Grafana k6 | v2.0.0 (used in this project) |
| OS | Windows 10/11 (used here) / macOS 12 / Ubuntu 20.04 |
| RAM | 512 MB (more helps at high VU counts) |
| Internet | Required (tests hit the live public API) |

---

## Installation

### Windows (winget) — the method I used
```powershell
winget install k6 --source winget
```

### macOS (Homebrew)
```bash
brew install k6
```

### Linux (Debian/Ubuntu)
```bash
sudo gpg -k
sudo gpg --no-default-keyring --keyring /usr/share/keyrings/k6-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5AD17C747E3415A3642D57D77C6C491D6AC1D69
echo "deb [signed-by=/usr/share/keyrings/k6-archive-keyring.gpg] https://dl.k6.io/deb stable main" | sudo tee /etc/apt/sources.list.d/k6.list
sudo apt-get update
sudo apt-get install k6
```

### Verify the installation
```bash
k6 version
```
This printed `k6.exe v2.0.0` on my machine, confirming it was ready.

---

## How to Run

I opened PowerShell in my project folder and ran each test:

```powershell
# Load Test
k6 run load-test.js

# Stress Test
k6 run stress-test.js

# Spike Test
k6 run spike-test.js
```

Each run printed a live progress bar and a full summary table of metrics at the end. To keep the raw data, the tests can also be run with `--out json=results/<name>.json`.

---

## Project Structure

```
MARIATULKAFTIAH BINTI OTHMAN/
│
├── README.md                          # This technical article
├── load-test.js                       # Load test (normal traffic, 20 VUs)
├── stress-test.js                     # Stress test (staircase to 300 VUs)
├── spike-test.js                      # Spike test (sudden burst to 300 VUs)
├── response-time-comparison.png       # Chart: response times
├── throughput.png                     # Chart: throughput
└── error-rate.png                     # Chart: error rate
```

---

## Test Types & Methodology

### Load Test
Simulates **normal, expected traffic**. k6 ramps up to 20 virtual users over 30 seconds, holds steady at 20 VUs for 1 minute, then ramps down. Each virtual user repeatedly performs a realistic mix of actions: mostly reading the list of posts (`GET /posts`) and occasionally creating a new post (`POST /posts`), with a 1-second "think time" between actions to mimic real human behaviour.

**Thresholds:** 95% of requests under 500 ms, and fewer than 1% failing.

### Stress Test
Pushes the system **beyond normal capacity** to find its breaking point. k6 climbs in a staircase — 50, then 100, then 200, then 300 virtual users, holding each level for one minute — before dropping back to zero to observe recovery.

**Thresholds:** 95% under 2000 ms, and fewer than 5% failing.

### Spike Test
Simulates a **sudden traffic burst**. k6 holds a calm baseline of 10 VUs, then jumps to 300 VUs within 10 seconds, holds the spike for one minute, drops sharply back to baseline, and watches for recovery.

**Thresholds:** 95% under 3000 ms, and fewer than 10% failing.

---

## Key Metrics Explained

| Metric | What it means | Why it matters |
|--------|---------------|----------------|
| `http_req_duration` | Total time for a request | The headline "how fast" number; usually read as the **p95** |
| `http_req_failed` | Percentage of requests that failed at the HTTP level | The reliability number |
| `custom_error_rate` | My own check: % of requests that returned a wrong status OR were too slow | Catches "slow but not broken" responses |
| `http_reqs` | Total requests, and requests per second | Measures **throughput** |
| `vus` | Active virtual users | Shows the load level |

> **p95 latency** means 95% of requests were faster than this value. It is preferred over the average because it captures the experience of the slowest (and most affected) users.

---

## Results & Analysis

All three tests were executed successfully on Windows 11 using k6 v2.0.0 against the live `/posts` endpoint. The table below presents the **actual measured results**.

### Summary of Results

| Test | Avg (ms) | p95 (ms) | Max | Throughput (req/s) | HTTP Errors | Check Errors | Max VUs | Total Requests |
|------|----------|----------|-----|--------------------|-------------|--------------|---------|----------------|
| **Load**   | 309.56 | 1050  | 3.00s | 11.55  | 0.00% | 8.40% | 20  | 1,404  |
| **Stress** | 278.75 | 933   | 8.97s | 101.70 | 0.00% | 0.37% | 300 | 30,548 |
| **Spike**  | 281.25 | 908.1 | 5.53s | 112.12 | 0.00% | 0.00% | 300 | 16,956 |

### Chart 1 — Response Time Comparison
![Response time comparison](response-time-comparison.png)

The most striking observation is that **average and p95 response times stayed remarkably flat** across all three tests (average hovering around 280–310 ms, p95 around 900–1050 ms), even as the load increased from 20 to 300 virtual users. What changed dramatically was the **maximum** response time, which jumped to 8.97 seconds under stress and 5.53 seconds under spike. This tells me that the typical user experience remained good, but a small number of requests suffered severe delays — a classic **tail-latency** problem.

### Chart 2 — Throughput
![Throughput](throughput.png)

Throughput scaled almost perfectly with load: from 11.55 req/s at 20 VUs to 101.70 req/s under stress and 112.12 req/s under spike. The API absorbed roughly **10× more traffic** without any HTTP-level failures, which indicates strong horizontal capacity on JSONPlaceholder's side.

### Chart 3 — Error Rate (Failed Checks)
![Error rate](error-rate.png)

Counter-intuitively, the **load test had the highest check-failure rate (8.40%)**, while the stress (0.37%) and spike (0.00%) tests were almost flawless. No test produced a single HTTP error (`http_req_failed` was 0.00% everywhere) — every failed check was caused by a response being *slower* than my threshold, not by the server actually rejecting a request.

### Discussion — Did the Hypothesis Hold?

My hypothesis was **partly wrong, in an interesting way**:

- I predicted normal load would be the *fastest and cleanest* test. In reality it had the *worst* p95 (1050 ms) and the *highest* failure rate (8.40%). I attribute this to a **cold-start effect**: the load test ran first, before TCP connections, DNS lookups, and any caching had "warmed up", so early requests were slow.
- I predicted errors would appear and grow as VUs climbed past 100–200. Instead, the API handled 300 concurrent users with **zero HTTP failures** and actually delivered *better* p95 latency than the warm-up load test.
- My spike prediction was the most accurate: the burst was survived cleanly (100% successful checks) and the only cost was a few very slow requests (5.53s max).

---

## Identified Bottlenecks

1. **Tail latency is the real bottleneck.** While the average and p95 are healthy, the maximum response time degrades badly under load — 3.0s (load) → 5.5s (spike) → 9.0s (stress). A small fraction of requests are being queued or delayed significantly during peak concurrency.
2. **Cold-start penalty.** The first test paid a one-time warm-up cost (connection setup, DNS, no cache), producing the misleadingly high 8.40% check-failure rate. This is a client/network-side bottleneck, not a server fault.
3. **No hard breaking point reached.** Even at 300 VUs the public API did not fail outright, so its true capacity ceiling is higher than this test exercised — a limitation of testing against a robust shared service.

---

## Recommendations

Based on the empirical evidence and industry best practice:

- **Address tail latency** with caching and a CDN for read-heavy endpoints like `GET /posts`, so the slowest requests are served from cache rather than queued.
- **Warm up connections** before measuring in future tests (a short "smoke" stage), so cold-start effects do not distort the headline numbers — a methodology improvement I would apply next time.
- **Add rate limiting and graceful degradation** in a real production API so sudden spikes return controlled responses instead of slow ones.
- **Scale horizontally** behind a load balancer to push the breaking point even higher.
- **Automate these k6 tests in a CI/CD pipeline** to catch performance regressions on every deployment.

---

## Conclusion

This project used Grafana k6 to subject the JSONPlaceholder `/posts` endpoint to three realistic traffic profiles — load, stress, and spike. The API proved impressively robust: it sustained a 10× increase in traffic (up to 300 concurrent virtual users and 112 requests per second) with **zero HTTP failures** across more than 48,000 total requests.

The key bottleneck was not outright failure but **tail latency** — a small number of requests slowed to as much as 9 seconds under heavy concurrency, even while the typical request stayed under one second. My original hypothesis that the system would degrade and fail under high load did not hold; instead, the most significant performance issue appeared as a **cold-start effect during the very first (load) test**, a valuable lesson in test methodology.

For a production system, I would recommend caching/CDN to flatten tail latency, connection warm-up to produce cleaner measurements, and continuous performance testing in CI/CD. Overall, the exercise demonstrated how empirical performance testing can overturn intuitive assumptions and reveal where a system's *real* weak points lie.

---

## Demo Video

▶️ **[Watch the walkthrough on YouTube](PASTE_YOUR_YOUTUBE_LINK_HERE)**

The video demonstrates the k6 installation, the test configuration, a live test execution, and a walkthrough of the most significant results.

---

*Submitted for **ITT440** — Universiti Teknologi MARA (UiTM) — 2026*
*Author: Mariatulkaftiah binti Othman*
