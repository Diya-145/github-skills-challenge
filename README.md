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
