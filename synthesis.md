# Synthesis

## Bottleneck hypothesis

The hypothesis was that the proxy hop contributes materially to authentication latency when delay is introduced. The intervention was a 300 ms delay for 20 seconds, with control and fault traffic both sent through the proxy. The single measurement that would change my mind is the injection-window p95: if it had not increased relative to the before window, the hypothesis would not be supported. It did increase from 217.22 ms (`n=270`) to 459.07 ms (`n=313`), while p99 increased from 268.00 ms to 483.99 ms. After the delay cleared, p95 was 176.68 ms (`n=693`) and p99 was 187.69 ms. This supports the claim that the injected proxy hop was a latency contributor under this workload.

## Database chaos hypothesis

I expected stopping PostgreSQL to produce authentication errors, especially for `user30`, which had not authenticated since the Keycloak restart. Instead, the warmed `user1` arm and the `user30` arm both recorded zero errors during the ten-second database outage. The most defensible interpretation is not that the service is generally resilient: the authentication requests in this experiment did not visibly require a live database connection during the measured window, possibly because of Keycloak's in-memory/cache state or the specific request path. The before/during/after phase statistics are in the corresponding JSON files under `faults/database/user1/` and `faults/database/user30/`.

## What cannot be concluded

These experiments cannot establish performance or failure behavior for other hosts, workloads, longer database outages, uncached users after a genuinely cold Keycloak start, or other database-dependent operations.
