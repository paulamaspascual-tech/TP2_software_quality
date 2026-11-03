# Fault card — 300 ms proxy-hop delay

- **Steady-state claim:** Under the proxy workload, the token endpoint has p95 latency below 200 ms and an error rate of at most 1%.
- **Fault:** Add 300 ms of delay per request at the proxy for 20 seconds using the bounded `/delay` control endpoint.
- **Workload:** 10 closed-loop Locust users, 0.1–0.3 s think time, 60 s duration, 5 s warm-up, 5 s request timeout, and `http://127.0.0.1:8081` for both control and fault traffic.
- **Blast radius:** Only requests traversing the local proxy are delayed. The Keycloak and PostgreSQL services are not intentionally changed.
- **Stop condition:** The delay expires automatically after 20 seconds; stop early with a zero-millisecond command if errors are sustained.
- **Analysis windows:** Control, injection, and recovery boundaries are recorded from the exact UTC injection and recovery timestamps and elapsed offsets in the complete raw CSV.
- **Observed:** The delay was injected at approximately elapsed 15 s for 20 s and then cleared. In the analysed windows, p95 was 217.22 ms before injection (`n=270`, 0 failures), 459.07 ms during injection (`n=313`, 0 failures), and 176.68 ms after recovery (`n=693`, 0 failures). The corresponding p99 values were 268.00 ms, 483.99 ms, and 187.69 ms. The full raw run is `full-run.csv`; each phase has a JSON analysis with bootstrap confidence intervals.
- **What this shows:** The 300 ms proxy-hop intervention increased p95 and p99 latency without causing errors in this run, and latency returned toward the control level after recovery. This tests an injected proxy-hop delay, not normal proxy bottleneck behavior or PostgreSQL network latency.
