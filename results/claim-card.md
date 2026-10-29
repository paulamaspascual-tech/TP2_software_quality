# Claim Card — Baseline

- **Operation:** `POST /realms/perf/protocol/openid-connect/token`
- **Workload:** 10 concurrent users; think time between 0.1 and 0.3 seconds; 35-second Locust run, with the first 5 seconds labelled as warm-up and excluded from analysis; 5-second request timeout; cache state not explicitly verified.
- **Statistic:** p95 latency and error rate. Percentiles use linear interpolation at position (n-1)*p. Error rate is failures divided by total measurement requests.
- **n observed:** 875, 878 and 884 measurement requests across three runs.
- **Threshold:** Error rate below 1% in each 35-second run.
- **Decision rule:** Ship if all three runs have an error rate below 1%; otherwise hold for investigation. The project team makes the final decision.
- **Limitation:** This claim applies only to the tested workload and environment. It does not establish performance on other machines or under different workloads. The IID bootstrap confidence intervals do not account for temporal dependence or host uncertainty. Cache state was not explicitly verified.
