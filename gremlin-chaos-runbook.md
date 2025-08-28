# Gremlin Chaos Engineering Runbook

## Environment
- On-prem Linux servers
- Oracle Database
- Spring Boot microservices
- Metrics: Micrometer + Prometheus
- Dashboards: Grafana

---

## Chaos Experiments

### 1. Network Latency
- **Gremlin Attack:** Latency
- **Target:** DB host
- **Command:** 2000ms latency for 60s
- **Expected Behavior:** 
  - App logs show DB timeout exceptions
  - Grafana shows spike in request latency
- **Recovery:** Traffic returns to normal once attack stops

### 2. Blackhole (Drop Packets)
- **Gremlin Attack:** Blackhole
- **Target:** DB host
- **Command:** Drop outbound port 1521 traffic
- **Expected Behavior:** 
  - DB queries fail with connection errors
  - Grafana shows sudden increase in DB error rate
- **Recovery:** Errors stop after attack ends

### 3. Packet Loss
- **Gremlin Attack:** Packet Loss
- **Target:** DB host
- **Command:** 40% loss for 2 min
- **Expected Behavior:** 
  - Retry logic kicks in
  - Error rate increases, but system partially recovers
- **Recovery:** Traffic stabilizes post attack

---

## Health Checks

- App responds on `/actuator/health`
- Prometheus metrics scrape success
- Grafana alert dashboard stable

---

## Rollback Plan
- Stop Gremlin experiment immediately
- Validate DB and app health
- Restart services if connections hang
