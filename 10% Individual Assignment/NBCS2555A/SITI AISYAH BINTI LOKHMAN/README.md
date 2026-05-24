# 🚀Performance Analysis of Fake Store API under Concurrent Load using Gatling
> Let’s learn how to perform professional API performance testing using Gatling Community Edition with real Load, Stress, and Spike testing scenarios.

> “Performance testing is not about breaking systems. It is about understanding how systems behave when reality happens.”

**Prepared by : SITI AISYAH BINTI LOKHMAN**  
**Student ID : 2024347199**  
**Group : CDCS2555A**

## 🌟 Introduction  
Performance testing helps us understand how a system behaves when many users access it at the same time.
This project explores how to perform professional API performance testing using Gatling Community Edition against the Fake Store API 4a public e-commerce API used to simulate realistic user traffic patterns.

## 🛠 Tools Used  
| Tool | Purpose |
|---|---|
| Java JDK 21 | Runtime environment |
| Apache Maven | Dependency management |
| Gatling Community Edition | Performance testing |
| Visual Studio Code | Source code editor |
| Fake Store API | API testing target |

## 📌 Project Overview  
In this project, the Fake Store API was used as the testing target to simulate real-world e-commerce traffic.
Three types of performance tests were implemented:  
| Test Type | Purpose |
|---|---|
| 🔹 Load Test | Simulate normal user traffic |
| 🔥 Stress Test | Push the system beyond limits |
| ⚡ Spike Test | Simulate sudden traffic spikes |  

The generated Gatling reports provide insights into:  
•	response time  
•	throughput  
•	active users  
•	request failures  
•	system stability  

## ⚙️ Test Setup  
| Parameter | Configuration |
|---|---|
| Tool | Gatling Community Edition 3.10.x |
| Language | Java Simulation |
| Target URL | https://fakestoreapi.com |
| Type of Test | Load test, Stress test, Spike test |
| Duration | • Load Test: ~30 Seconds <br> • Stress Test: ~60 Seconds <br> • Spike Test: ~5–10 Seconds |
| Total Virtual Users | • Load Test: 50 Users <br> • Stress Test: 1000 Users <br> • Spike Test: 1000 Concurrent Users |
| System Environment | Windows 11, Java JDK 21.0.9, 8 GB RAM |
| Generated Reports | HTML Performance Report (`target/gatling/index.html`) |
| Scenario | 1️⃣ Get Products → 2️⃣ View Product Details → 3️⃣ View Categories |

## 🧾 Script/Command  
### Load Test Script
> run the script at **Visual Studio Code** 
```java
import io.gatling.javaapi.core.*;
import io.gatling.javaapi.http.*;
import static io.gatling.javaapi.core.CoreDsl.*;
import static io.gatling.javaapi.http.HttpDsl.*;
public class LoadTest extends Simulation {

        // URL Target
    HttpProtocolBuilder httpProtocol = http
        .baseUrl("https://fakestoreapi.com")
        .acceptHeader("application/json");
        // Header
    ScenarioBuilder scn = scenario("Load Test")
        // Open products
        .exec(
            http("Open Products Page")
                .get("/products")
                .check(status().is(200))
        )
        .pause(2)
        // View product details
        .exec(
            http("View Product 1")
                .get("/products/1")
                .check(status().is(200))
        )
        .pause(2)
        // View categories
        .exec(
            http("View Categories")
                .get("/products/categories")
                .check(status().is(200))
        );
    {
        setUp(
            scn.injectOpen(
                rampUsers(50).during(30)
            )
        ).protocols(httpProtocol);
    }
}
```
### Stress Test Script
```java
import io.gatling.javaapi.core.*;
import io.gatling.javaapi.http.*;
import static io.gatling.javaapi.core.CoreDsl.*;
import static io.gatling.javaapi.http.HttpDsl.*;
public class StressTest extends Simulation {

        // URL Target
    HttpProtocolBuilder httpProtocol = http
        .baseUrl("https://fakestoreapi.com")
        .acceptHeader("application/json");
        // Header
    ScenarioBuilder scn = scenario("Stress Test")
        // Open Products
        .exec(
            http("Get Products")
                .get("/products")
                .check(status().is(200))
        )
        .pause(1)
        // View product details
        .exec(
            http("View Product")
                .get("/products/1")
                .check(status().is(200))
        )
        .pause(1)
        // View categories
        .exec(
            http("View Categories")
                .get("/products/categories")
                .check(status().is(200))
        );
    {
        setUp(
            scn.injectOpen(
                rampUsers(1000).during(60)
            )
        ).protocols(httpProtocol);
    }
}
```
### Spike Test Script
```java
import io.gatling.javaapi.core.*;
import io.gatling.javaapi.http.*;
import static io.gatling.javaapi.core.CoreDsl.*;
import static io.gatling.javaapi.http.HttpDsl.*;
public class SpikeTest extends Simulation {

        // URL Target
    HttpProtocolBuilder httpProtocol = http
        .baseUrl("https://fakestoreapi.com")
        .acceptHeader("application/json");
        // Header
    ScenarioBuilder scn = scenario("Spike Test")
        // Open Products
        .exec(
            http("Get Products")
                .get("/products")
                .check(status().is(200))
        )
        .pause(1)
        // View product details
        .exec(
            http("View Product")
                .get("/products/1")
                .check(status().is(200))
        )
        .pause(1)
        // View categories
        .exec(
            http("View Categories")
                .get("/products/categories")
                .check(status().is(200))
        );
    {
        setUp(
            scn.injectOpen(
                nothingFor(5),
                atOnceUsers(1000)
            )
        ).protocols(httpProtocol);
    }
}
```
### Command Prompt
```cmd 
cd (path file location)
mvn gatling:test
```
![Image Description](https://github.com/cacaerssss/Gat_shine/blob/6f3289b749983a18b881134e959d5a46befdae73/images/command%20prompt.png)  
![Image Description](https://github.com/cacaerssss/Gat_shine/blob/6f3289b749983a18b881134e959d5a46befdae73/images/command%20prompt%201.png)  

## 📊 Comparative of Test Result Summary  
| Performance Metric | Load Test (Baseline Traffic) | Stress Test (Sustained Volume) | Spike Test (Sudden Traffic Burst) | Description |
|---|---|---|---|---|
| Total Test Volume | 150 requests | 1,000 requests | 3,000 requests | Measures total data capacity processed during the test duration |
| Throughput (Mean Cnt/s) | 4.41 req/s | 15.87 req/s | 166.67 req/s | Shows overall system ingestion speed. spike scales ~37x over baseline |
| Error Rate (% KO) | 0% (0 / 150) | 0% (0 / 1,000) | 0% (0 / 3,000) | Excellent stability with 100% request success across all loads |
| Min Response Time | 213 ms | 283 ms | 219 ms | Best case latency under ideal conditions (no queue delay) |
| 50th Percentile (Median) | 236 ms | 329 ms | 886 ms | Typical user experience: Stable under load, increases under spikes |
| 75th Percentile | 331 ms | 343 ms | 4,405 ms | Shows performance degradation under higher resource pressure |
| 95th Percentile | 476 ms | 448 ms | 11,795 ms | SLA indicator : spike test shows significant tail latency |
| 99th Percentile | 505 ms | 1,034 ms | 13,938 ms | Worst-case near limit behavior under extreme load |
| Max Response Time | 505 ms | 1,107 ms | 14,387 ms | Peak latency: reflects queue saturation under stress |
| Average Response Time | 286 ms | 347 ms | 2,878 ms | Mean latency: heavily skewed by spike test tail delays |
| Standard Deviation | 80 ms | 63 ms | 3,786 ms | Consistency indicator: spike test shows high variability |  

## 🔍 The Analysis  
1. What the App Does Great: It is unbreakable  
Across all three tests handling more than 4,000 total requests, the application had a 0% error rate. This means the server never crashed, never threw a "Server Error (500)" page, and never dropped a connection. It is show the pages is stable.

2. Where the App Bottlenecks: It forces a waiting line  
When the sudden Spike traffic hit, the server didn't crash. Instead, it created a virtual waiting line (a request queue).  
The first few users got in instantly (under 1 second).  
The users at the back of the line had to wait up to 11 to 14 seconds for their page to load.

3. The Trouble Spot: The Products Page  
The data shows that the Open Products Page is the heaviest part of your app. Under heavy traffic, loading products causes the biggest traffic jams. Lightweight pages (like viewing a single product) stay fast.

## 🔥 Raw Data
### 🧬 Load Test (Baseline Traffic)  
🟢 Performance: All responses completed well under 600 ms—excellent processing speeds.  
🟢 Stability: 0% error rate across 150 total requests.  
🟢 Consistency: Low standard deviation (80 ms) indicates reliable, predictable server behavior.  
🟢 Scalability: Maintained fast performance profiles during standard concurrency ramps.
| Chart | Description |
|---|---|
| ![Image Description](https://github.com/cacaerssss/Gat_shine/blob/b556584cb68b3e514ba5dca25d6c411ca14bc9dc/images/Global%20Gatling%20Documentation%20LT.png) | 150 total request in Global Gatling Data |
| ![Image Description](https://github.com/cacaerssss/Gat_shine/blob/b556584cb68b3e514ba5dca25d6c411ca14bc9dc/images/concurrent%20user%20LT.png) | Concurrent user for load test |
| ![Image Description](https://github.com/cacaerssss/Gat_shine/blob/b556584cb68b3e514ba5dca25d6c411ca14bc9dc/images/Request%20%26%20response%20LT.png) | Number of request and response/sec |

### 🧠 Stress Test (Sustained Volume)  
🟢 Performance: 95% of traffic completed under 450 ms, with only edge cases passing 1 sec, exceptional sustained speed.  
🟢 Stability: 0% error rate across 1,000 total requests.  
🟢 Consistency: Remarkably low deviation (63 ms), showcasing high predictability under load.  
🟢 Scalability: Smoothly maintained performance as heavy sustained traffic pushed throughput to ~16 requests per second.  
| Chart | Description |
|---|---|
| ![Image Description](https://github.com/cacaerssss/Gat_shine/blob/b556584cb68b3e514ba5dca25d6c411ca14bc9dc/images/Global%20Gatling%20Documentation%20STT.png) | 1,000 total request in Global Gatling Data |
| ![Image Description](https://github.com/cacaerssss/Gat_shine/blob/b556584cb68b3e514ba5dca25d6c411ca14bc9dc/images/concurrent%20user%20stt.png) | Concurrent user for stress test |
| ![Image Description](https://github.com/cacaerssss/Gat_shine/blob/b556584cb68b3e514ba5dca25d6c411ca14bc9dc/images/Request%20%26%20response%20stt.png) | Number of request and response/sec |

### 📈 Spike Test (Sudden Traffic Burst)  
🟢 Performance: Average response times scaled to 2.8 seconds, with the slowest transactions stalling up to 14.3 seconds due to server backpressure.  
🟢 Stability: Outstanding resilience; 0% error rate across 3,000 requests during a massive sudden traffic surge.  
🟢 Consistency: Highly volatile consistency (3,786 ms deviation) as resources queued up behind the intense burst.  
🟢 Scalability: Demonstrates high survival capability, keeping the application alive and operational at 166+ requests per second without throwing server failures.  

| Chart | Description |
|---|---|
| ![Image Description](https://github.com/cacaerssss/Gat_shine/blob/b556584cb68b3e514ba5dca25d6c411ca14bc9dc/images/Global%20Gatling%20Documentation%20SPT.png) | 3,000 total request in Global Gatling Data |
| ![Image Description](https://github.com/cacaerssss/Gat_shine/blob/b556584cb68b3e514ba5dca25d6c411ca14bc9dc/images/concurrent%20user%20spt.png) | Concurrent user for spike test |
| ![Image Description](https://github.com/cacaerssss/Gat_shine/blob/b556584cb68b3e514ba5dca25d6c411ca14bc9dc/images/Request%20%26%20response%20spt.png) | Number of request and response/sec |

## 🧠 What This Data Means (Interpretation)  
Basically, these 3 tests show that our web app is super stable, but it definitely has a speed limit when too many people try to use it at the exact same time.  
- It’s literally unbreakable: Out of all 4,150 requests we threw at it across the tests, we got a perfect 0% error rate. The server never crashed, it didn't throw any random 500 errors, and it didn't drop a single connection. That's a huge win for stability.
- It handles everyday traffic perfectly: Going from normal traffic (Load Test) to a busy, constant rush hour (Stress Test) barely changed anything. Traffic jumped by 260%, but the average response time only went up by a tiny 61 ms (from 286 ms to 347 ms). This means under steady load, the server is super optimized and handles requests like a breeze.
- The traffic jam during spikes: The real issue happened during the Spike Test. When a massive burst of traffic hit all at once, the server didn't crash, but it got super slow because it forced users into a waiting line (a queue). Half the users got their page fast, but the people at the back of the line had to wait anywhere from 11 to 14 seconds, which would totally make a real user give up and close the tab.

## 📚 Conclusion and Improvement 
### ❓ How We Can Improve It
- Fix the Products Page first (Use Caching): The raw data showed that Open Products Page / Get Products was the main bottleneck, taking an average of over 7 seconds to load during the spike. We should set up a cache, so the server can just serve a saved copy of the products page instantly, instead of building it from scratch every time.
- Make Auto-Scaling Faster: We need to tweak our cloud settings (like AWS) to spin up backup servers the exact second it sees a jump in requests per second, rather than waiting for the server's CPU to get completely maxed out first.
- Tune the Database Connection Pool: We should increase the maximum database connection limits so more backend worker threads can talk to the database at the same time, clearing out that bottleneck line much faster.

### 🏆 Conclusion  
The good news is that the app has an amazing, rock-solid foundation. It handles standard and heavy traffic easily without buckling. The bad news is that its scaling is too slow. Instead of spawning more server power instantly when a spike happens, it just makes users wait in a slow line. If we launched this in production right now, a sudden surge of users would cause huge lag.

## 📹 Video Demostration
