<img src="./imgs/zelros.svg" width="250">


# 400: Always monitor

It is important to distinguish alerting, monitoring and logging.

Alerting is the process of detecting failure and triggering issue for the IT team.

Monitoring is the process of collecting KPI to watch application health.

Logging is the process of tracing every useful information to analyze and solve past issue.


## 410: Alert

### 411: Rely on a probe endpoint

Each API has a probe endpoint `/probe`, which tests if the system is functional. 
The heartbeat system calls the endpoint at regular intervals.

```
GET /probe

HTTP 200 OK
```

If the system is fully functional, the endpoint returns a **200** status code.

Otherwise, a **500** status code is answered, and an alert is triggered and sent to the IT team.


### 412: Trigger a functional test

The probe endpoint calls the API itself, over HTTP. It simulates an external client which calls the API.

The probe achieves a business E2E test, not a technical test. The goal is to validate that the API is up and running for client.

The test shouldn't last too long for the heartbeat system.


## 420: Monitor SLA


## 430: Log business informations


## 440: Log technical informations
