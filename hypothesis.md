# Hypothesis — Proxy latency injection

- **Suspect:** The proxy hop adds latency to authentication requests when an additional delay is introduced.
- **Predicted signal:** Compared with the control run, p95 and p99 response times will increase during the injected delay. The error rate may remain unchanged if requests complete within the 5-second timeout.
- **Intervention:** Inject a 300 ms delay at the proxy for 20 seconds using the provided `/delay` endpoint. Keep the workload, request, and proxy route unchanged.
- **Falsifier:** If p95 and p99 do not increase during the injection compared with the control measurements, the prediction is not supported.
- **Workload:** 10 concurrent users, 0.1–0.3 seconds think time, 60-second run, 5-second warm-up, and 5-second request timeout. Use `http://127.0.0.1:8081` for both control and fault runs.
- **Comparison:** Analyse the control, injection, and recovery periods separately, using the same percentile estimator. Record the number of requests and error rate in each period.
- **Limitation:** This tests an intentionally injected proxy delay in the local teaching stack. It does not establish that the proxy is a bottleneck under normal conditions, and differences between periods may also be affected by time-dependent workload or host conditions.
