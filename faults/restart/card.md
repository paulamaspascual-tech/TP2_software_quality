# Fault card — Keycloak restart

- **Steady-state claim:** Under the specified workload, the token endpoint has p95 latency below 200 ms and an error rate of at most 1%.
- **Fault:** Restart the Keycloak container using `docker compose restart kc`.
- **Workload:** 10 concurrent users, 0.1–0.3 s think time, 60 s total duration, 5 s warm-up and a 5 s request timeout.
- **Endpoint:** `http://127.0.0.1:8080/realms/perf/protocol/openid-connect/token`.
- **Blast radius:** Authentication requests routed to Keycloak may fail during the restart and recovery period. The PostgreSQL container remains running.
- **Stop condition:** Stop the load and inspect logs if Keycloak does not recover within 60 s.
- **Observed:** The full run recorded 1,776 requests, including warm-up. Among the 1,629 measurement requests, 130 failed (7.98%). The failures were `RemoteDisconnected` and `ConnectionRefusedError`. Using approximate time windows derived from the request trace, the before phase had 0 errors out of 366 requests; the during phase had 130 errors out of 137 requests (94.89%); and the after phase had 0 errors out of 1,126 requests.
- **Latency:** Before the fault, p95 was 169.84 ms; during the fault, p95 was 127.85 ms; after recovery, p95 was 161.96 ms. The lower p95 during the fault is not evidence of improved performance because most requests failed quickly.
- **What it shows:** Restarting Keycloak caused a temporary loss of authentication availability. Successful requests resumed after recovery, and the before/after phases had no observed errors. The overall run exceeded the 1% error-rate threshold, so the steady-state claim did not hold over the full fault-injection run.
- **Phase-boundary limitation:** The before/during/after windows are approximate, based on elapsed request times; they do not record the exact restart command timestamp. The observed failure interval is concentrated around 17.77–20.38 s.
- **Statistical limitation:** The percentile bootstrap assumes IID requests and does not capture temporal dependence or host uncertainty.