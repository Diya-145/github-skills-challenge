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

-------------------------TASK 3-------------------------

The provided `AnomalyDetector` was used to analyse all 10 operational records.
The detector checks:
- Response time above 500 ms
- CPU usage above 80%
- Memory usage above 80%
- Log level equal to `ERROR`

=> Detected Anomalies
| Timestamp | Metrics and Log Information | Detection Reasons |
|---|---|---|
| 2026-09-20T10:05:00 | Response time: 610 ms; log level: ERROR; message: Payment service timeout | High response time; Error log detected |
| 2026-09-20T10:06:00 | Response time: 640 ms; CPU: 94%; memory: 91%; log level: ERROR; message: Database connection timeout | High response time; High CPU utilization; High memory utilization; Error log detected |
The detector classified 8 records as normal and 2 records as anomalous. The normal records had response times between 120 ms and 150 ms, CPU usage between 42% and 50%, memory usage between 51% and 57%, and successful `INFO` messages.
No expected anomalies were missed, and no normal observations were incorrectly flagged.
Each anomaly includes the timestamp, service name, detection reasons, and original source record. This provides enough information to understand why the observation was flagged.

=> Detection Limitation
The detector uses fixed thresholds. It may not detect gradual changes in normal behaviour or adapt to different service baselines. A possible improvement would be to calculate a baseline from historical data and detect significant deviations from that baseline.

-----------------------------TASK 3 COMPLETED--------------------------

---------------------------TASK 4-----------------------------

The AIOps event flow was verified using the provided pipeline.
The anomaly detector identified two anomalies at:
- `2026-09-20T10:05:00`
- `2026-09-20T10:06:00`
Each detected anomaly was passed to the `EventProducer`. The producer published the event to the in-memory `service-events` topic. The `EventConsumer` was connected to that same topic and received both events.
The consumer returned the events to `run_pipeline()`, which passed them to the final AIOps output. The execution result was:

- Records processed: 10
- Anomalies detected: 2
- Events consumed: 2

The event flow was:

Anomaly Detector -> Event -> Producer -> service-events Topic -> Consumer -> AIOps Output

The producer publishes messages, the topic stores messages in memory, the consumer retrieves messages, and the pipeline displays the processed events.

-----------------------TASK 4 COMPLETED--------------------

-----------------------TASK 5--------------------------

Two workflow issues were identified and corrected.

=> Issue 1: Error log detection

The anomaly detector originally checked for `WARNING` log entries, but the provided operational data uses `ERROR` for the timeout records. The affected component was `src/anomaly_detector.py`.
The condition was corrected to detect `ERROR` log levels. After the correction, the records at 10:05 and 10:06 were identified with the reason `Error log detected`.

=> Issue 2: Producer and consumer topics

The producer published events to the `service-events` topic, while the
consumer was connected to a separate `anomaly-events` topic. Because the topics were separate in-memory objects, the consumer received zero events.
The affected component was `src/aiops_pipeline.py`. The consumer was corrected to use the same `producer_topic` as the producer.
After the correction, the workflow produced:
- Records processed: 10
- Anomalies detected: 2
- Events consumed: 2
The complete corrected flow is:

Anomaly Detector -> Event -> Producer -> service-events Topic -> Consumer -> AIOps Output
-------------------------TASK 5 COMPLETED----------------------


------------------------TASK 6-------------------

The complete AIOps pipeline was executed using:

```bash
PYTHONPATH=src python3 src/aiops_pipeline.py
```

Execution result:

- Operational records processed: 10
- Anomalies detected: 2
- Events published: 2
- Events consumed: 2
- Events processed successfully: 2

The detected issues occurred at:

- `2026-09-20T10:05:00`: payment service timeout, high response time, and `ERROR` log
- `2026-09-20T10:06:00`: database connection timeout, high response time, high CPU, high memory, and `ERROR` log

The verified end-to-end flow was:

Operational Data -> Anomaly Detection -> Event -> Producer -> Topic -> Consumer -> AIOps Output

The final output successfully represented the detected operational issues.

-------------------------TASK 6 COMPLETED--------------------------

-------------------------TASK 7-------------------------

=> Reproduction Steps

1. Clone or fork the repository.
2. Open the repository in GitHub Codespaces or VS Code.
3. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Run the complete AIOps pipeline:

   ```bash
   PYTHONPATH=src python3 src/aiops_pipeline.py
   ```

5. Run the tests:

   ```bash
   PYTHONPATH=src python3 -m pytest
   ```

6. Confirm that 10 records are processed, 2 anomalies are detected, and 2 events are consumed.

The project monitors a simulated `payment-service`. AIOps analyses its metrics and logs, detects abnormal behaviour, creates anomaly events, publishes them to an in-memory topic, consumes them, and displays the final result.
The main operational data is stored in `data/service_data.json`. Metrics include response time, CPU usage, and memory usage. Log information includes the log level and message. The timestamps are recorded at one-minute intervals and allow metric and log activity to be correlated.

The normal records contain successful `INFO` messages and low response time, CPU, and memory values. The unusual records occur at 10:05 and 10:06 and contain timeout errors, high response times, and, at 10:06, high CPU and memory usage.

The anomaly detector identified both expected anomalies and did not incorrectly flag any normal records. The event flow uses the `EventProducer` to publish events, the `EventTopic` to store them, and the `EventConsumer` to retrieve them for the final AIOps output.

Two issues were corrected during the investigation. The detector was changed to recognise `ERROR` log entries because the supplied data did not use `WARNING`. The consumer was connected to the same topic used by the producer, so the anomaly events could travel through the complete workflow.

One limitation is that the detector uses fixed thresholds. It may not adapt to changing service baselines. A possible improvement would be to calculate historical baselines and detect significant deviations from them.

-------------------------TASK 7 COMPLETED--------------------------

-------------------------TASK 8-------------------------

The provided validation was executed with:

```bash
PYTHONPATH=src python3 -m pytest

-------------------TASK 8 Completed------------------