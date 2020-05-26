<img src="./imgs/zelros.svg" width="250">

# 400: Always monitor

It’s impossible to manage a service correctly without understanding which behaviors really matter for that service 
and how to measure and evaluate those behaviors. To this end, we would like to define and deliver a given level of service to our users.
We define service **level indicators (SLIs)**, **objectives (SLOs)**, and **agreements (SLAs)**. 
These measurements describe basic properties of metrics that matter, what values we want those metrics to have, and how we’ll react if we can’t provide the expected service.
Ultimately, choosing appropriate metrics helps to drive the right action if something goes wrong.

- Indicators

An SLI is a service level indicator—a carefully defined quantitative measure of some aspect of the level of service that is provided.
SLI includes **request latency** (how long it takes to return a response to a request) and **error rate** expressed as a fraction of all requests received, 
and **system throughput**, typically measured in requests per second.

These indicators are used to compute the **availability** indicators which indicates the fraction of the time that a service is usable. 

- Objectives

An SLO is a service level objective: a target value or range of values for a service level that is measured by an SLI. 
A natural structure for SLOs is thus **SLI ≤ target**, or **lower bound ≤ SLI ≤ upper bound**.

- Agreements

Finally, SLAs are service level **agreements**: an explicit or implicit contract with your users that includes consequences of meeting (or missing) the SLOs they contain.
The consequences are most easily recognized when they are financial—a rebate or a penalty—but they can take other forms. 

## Metrics

To be able to implement these indicators, every service shall implement a specific handler to collect all the metrics.
This handler shall be accessible on **/metrics** internally only. **It shall not be exposed on the internet**

The format used for these metrics is Prometheus metrics / OpenMetrics format

Prometheus metrics text-based format is line oriented. Lines are separated by a line feed character (\n). 
The last line must end with a line feed character. Empty lines are ignored.

A metric is composed by several fields:
- Metric name
- Any number of labels (can be 0), represented as a key-value array
- Current metric value
- Optional metric timestamp

A Prometheus metric can be as simple as: 
```
http_requests 2
```
Or, including all the mentioned components:
```
http_requests_total{method="post",code="400"}  3   1395066363000
```

The following metrics shall be implemented
- up: A gauge (0 or 1) to indicate the service status
- http_request_duration_seconds_bucket an histogram with the number of request per duration with labels
    - le: duration time limit (in second)
    - status_code: HTTP response code
    - method: HTTP method
    - path: HTTP url path
- http_request_duration_seconds_sum a sum of the requests durations with labels
    - status_code: HTTP response code
    - method: HTTP method
    - path: HTTP url path
- http_request_duration_seconds_count a count od the requests with labels
    - status_code: HTTP response code
    - method: HTTP method
    - path: HTTP url path
    
For example:
```
http_request_duration_seconds_bucket{le="0.25",status_code="200",method="POST",path="/analyze"} 0
http_request_duration_seconds_bucket{le="0.5",status_code="200",method="POST",path="/analyze"} 0
http_request_duration_seconds_bucket{le="1",status_code="200",method="POST",path="/analyze"} 0
http_request_duration_seconds_bucket{le="3",status_code="200",method="POST",path="/analyze"} 0
http_request_duration_seconds_bucket{le="5",status_code="200",method="POST",path="/analyze"} 0
http_request_duration_seconds_bucket{le="7",status_code="200",method="POST",path="/analyze"} 1
http_request_duration_seconds_bucket{le="10",status_code="200",method="POST",path="/analyze"} 1
http_request_duration_seconds_bucket{le="13",status_code="200",method="POST",path="/analyze"} 1
http_request_duration_seconds_bucket{le="15",status_code="200",method="POST",path="/analyze"} 2
http_request_duration_seconds_bucket{le="20",status_code="200",method="POST",path="/analyze"} 2
http_request_duration_seconds_bucket{le="+Inf",status_code="200",method="POST",path="/analyze"} 5
http_request_duration_seconds_sum{status_code="200",method="POST",path="/analyze"} 103.941766924
http_request_duration_seconds_count{status_code="200",method="POST",path="/analyze"} 5

up 1
```
The buckets definition shall be configured to fit the service response times

Use the prometheus client library to implement your metrics [Prometheus Client library](https://prometheus.io/docs/instrumenting/clientlibs/)
Several examples can be found in the codebase of Zelros for nodejs, Flask or Tornado servers.


## 410: Test API contract


## 420: Monitor SLA


## 430: Log business informations


## 440: Log technical informations
