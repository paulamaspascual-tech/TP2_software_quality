# Claim Card — Keycloak token endpoint baseline

- **Claim:** Under the specified local workload, p95 latency is below 200 ms and the error rate is no greater than 1%.
- **Operation:** `POST /realms/perf/protocol/openid-connect/token`
- **Workload:** 10 concurrent users, 0.1–0.3 s think time, 35 s run, 5 s warm-up, and 5 s request timeout.
- **Identity and cache state:** Every request authenticates as `user1`; bearer tokens are not reused. This is not a cold-start experiment.
- **Statistic:** p50, p95, p99, and mean latency plus error rate. Percentiles use linear interpolation at `(n-1)*p`.
- **n observed:** 875, 878, and 884 measurement requests across the three runs.
- **Threshold:** p95 < 200 ms and error rate <= 1% in every run.
- **Decision rule:** Support the claim if all three baseline runs meet both thresholds; otherwise hold for investigation. The decision applies only to this local setup.
- **Observed:** All three runs had zero failures. p95 values were 154.79 ms, 155.55 ms, and 155.54 ms.
- **Uncertainty:** Each percentile has a 95% IID bootstrap confidence interval from 1,000 resamples using seed 8371. The bootstrap does not capture temporal dependence or host-to-host uncertainty.
- **Limitations:** These results do not establish performance under other workloads, machines, deployments, or cache states.
