# AIMAN HAKIM BIN BADRUL ZAMAN
1.0 - INTRODUCTION 

Target API : https://fakestoreapi.com 
Tool: Apache JMeter 5.6.3 - CMD
Tester : AIMAN HAKIM BIN BADRUL ZAMAN (SID : 2024130077)

2.0 -  Why Apache JMeter? 
Jmeter offers a perfect environment for beginners while also providing a large space for skills to grow. It is a low skill floor and high skill ceiling tools. 
Demonstrating spike, load and soak test easily
Free to use app and easy to set up as well as run perfectly on Window

3.0 - Test Environment Setup and Methodology
3,1 Test Environment Setup (visual guide)
For the environment setup, the user can watch the video below.

https://youtu.be/qY2wVylRsRM?si=R5SvOqhR6YLrZSCC 

3.2 Methodology

Apache JMeter operates on GUI. Therefore, no codes are required and just need to fill in the settings for the test template.


COMMON SETTINGS
Server Name
fakestoreapi.com 
Protocol
HTTPS
HTTP Method
GET
Path
/products
Content - Type Header
Application / json
Timer
1000 ms (1 seconds)
Listeners
Summary report + Aggregate report



SETTINGS OVERVIEW


Load Test
Soak Test
Spike Test 
Thread group name
Load Test
Soak Test
Normal Load
Spike Load
Number of thread
300
150
50
400
Ramp-Up periods (seconds)
60
60
30
5
Duration (seconds)
900 (15 min)
7200 (2 hrs)
600 (10 min)
180 (3 min)
Loop count
Forever
Scheduler
Enabled
Target Endpoint
/products
Timer
1000 ms
Test File Name
LoadTest.jmx
SoakTest.jmx
SpikeTest.jmx







4.0 Raw Data Presentation
4.1 Load Test

Performance index




Performance index points at 0.994 which then can be rated at excellent performance. This index measures user satisfaction and getting 0.994 means that the user is highly satisfied with the API performance.


Statistics
The Load Test with 300 concurrent users achieved an average response time of 248 ms, with 95% of requests completing under 263 ms. The API processed 231.75 transactions per second with a near-perfect error rate of 0.00%

Load Test Overview

fakestoreapi.com excellently handles load test with near-perfect error rate of 0.00%, proving its reliable performance for free public API.






4.2 Soak Test

Performance Index



An Apdex score of 0.975 is very high. This means users would be highly satisfied with the performance even after running continuously for 2 hours.

Statistics
fakestoreapi.com shows a very stable performance with 0 errors throughout 2 hours. The API maintaining a solid average response time of 347 ms and shows a very good endurance for a free public API


Soak Test Overview

The Soak Test with 150 concurrent users ran successfully for 2 hours. The API demonstrated excellent stability with 0% error rate and processed 798,152 requests. The average response time remained consistent at 347 ms, and the Apdex score of 0.975 indicates strong user satisfaction even under sustained load.













4.3 Spike Test

Performance Index



Apdex score at 0.988 which shows the API excellent performance in taking sudden burst of users


Statistics 
The API handled the Spike Test very well, handling a sudden burst up to 400 users with zero errors, an excellent average response time of 271 ms, and strong throughput of 131 requests per second.


Spike Test Overview

The Spike Test with a sudden burst of 400 handled well by the API. It shows that the API is reliable and consistent with an average of 270 ms response time, and the Apdex score of 0.988 indicates that the user is very satisfied with the performance.

5. 0 Conclusion

	The fakestoreapi.com demonstrated strong and reliable performance across all three performance tests. It successfully handled a steady load of 300 users, a sudden spike up to 400 concurrent users, and a prolonged endurance test of 150 users for 2 hours with zero errors (0.00% error rate) in all scenarios.


Response times remained consistently good, with average times ranging between 248 ms to 347 ms, and excellent Apdex scores between 0.975 – 0.988, indicating high user satisfaction. The API showed good stability with no major degradation during long-duration testing and effectively managed sudden traffic bursts.
