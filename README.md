# GitHub Challenge

<img src="https://octodex.github.com/images/Professortocat_v2.png" align="right" height="200px" />

Hey there!

Your challenge is ready.
Follow the instructions provided for this challenge and complete the required tasks in this repository.

Make sure your work is committed and pushed to your repository before submission.

Good luck!


---

&copy; 2025 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)


--------------------TASK 1------------------------

This project monitors a simulated `payment-service`. The service produces operational telemetry containing response times, CPU usage, memory usage, log levels, and log messages.
The operational problem is identifying abnormal service behaviour, such as slow payment requests, high CPU or memory usage, and timeout-related log messages. These conditions can indicate performance degradation or a possible service failure.

AIOps is used in this assessment to analyse operational data, detect
anomalies, generate anomaly events, publish those events to an in-memory topic, consume the events, and produce a final operational result. The workflow is:
Operational Data -> Anomaly Detection -> Event Generation -> Producer -> Topic -> Consumer -> AIOps Output

-----------------------TASK 1 COMPLETED-----------------------

--------------------------TASK 2----------------------------

The operational data is stored in `data/service_data.json` and contains
observations from the simulated `payment-service`.

=> Metrics
The metric fields are:
- `response_time_ms`: payment response time in milliseconds
- `cpu_percent`: CPU utilisation percentage
- `memory_percent`: memory utilisation percentage
The `service` field identifies the monitored service.

=> Log Information
The log fields are:
- `log_level`: severity of the log entry, such as `INFO` or `ERROR`
- `message`: description of the service activity or problem

=> Timestamps
The `timestamp` field records when each observation occurred. The values use the ISO-style format `YYYY-MM-DDTHH:MM:SS`. The records are ordered chronologically at one-minute intervals, covering 10:00 through 10:09 on 2026-09-20. Timestamps allow metric values and log messages to be correlated with events at a specific time.

=> Normal Behaviour
Eight observations appear normal: 10:00 through 10:04 and 10:07 through
10:09. These records have response times between 120 ms and 150 ms, CPU usage between 42% and 50%, memory usage between 51% and 57%, `INFO` log levels, and successful payment-processing messages.

=> Unusual Behaviour
The observation at 10:05 is unusual. It has a response time of 610 ms, an `ERROR` log level, and the message `Payment service timeout`.
The observation at 10:06 is also unusual and more severe. It has a response time of 640 ms, CPU usage of 94%, memory usage of 91%, an `ERROR` log level, and the message `Database connection timeout`.
These two observations suggest a temporary performance problem followed by high resource utilisation and a database connection problem.

---------------------TASK 2 COMPLETED-------------------
