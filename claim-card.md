# Claim card — Keycloak token endpoint baseline

- **Claim:** Under the specified local workload, the Keycloak token endpoint responds with p95 latency below 200 ms and an error rate no greater than 1%.
- **Operation:** POST `/realms/perf/protocol/openid-connect/token`.
- **Workload:** 10 concurrent Locust users, spawning at 10 users/second, with a random think time between 0.1 and 0.3 seconds.
- **Duration:** 35 seconds per run, with the first 5 seconds excluded as warm-up.
- **Endpoint:** `http://127.0.0.1:8080`.
- **Identity and cache state:** Requests authenticate as `user1`; each request authenticates again rather than reusing a bearer token. The realm already exists and its password policy is configured by the seed script. This is not a cold-start experiment.
- **Primary statistic:** p95 response time, calculated using linear interpolation at `(n-1)*p`.
- **Thresholds:** p95 < 200 ms; error rate <= 1% of all measured requests in each run.
- **Decision rule:** The claim is supported for this workload if all three baseline runs meet both thresholds. Otherwise, the claim is not supported. This decision applies only to the measured local setup.
- **Observed baseline:** 0 failed requests in all three runs. Sample sizes: 875, 878, and 884 measured requests. p95 values: 154.79 ms, 155.55 ms, and 155.54 ms, respectively.
- **Uncertainty:** Report the p50, p95, and p99 with their 95% bootstrap confidence intervals for each run. The bootstrap resamples individual requests (1,000 repetitions, seed 8371).
- **Limitations:** The IID request bootstrap does not capture temporal dependence or uncertainty across hosts/runs. These local results do not establish performance under other workloads or deployment environments.
