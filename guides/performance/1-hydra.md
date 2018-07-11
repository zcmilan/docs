# ORY Hydra Performance Benchmarks

In this document you will find benchmark results for different endpoints of ORY Hydra. All benchmarks are executed
using [rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks run against the in-memory storage
adapter of ORY Hydra. These benchmarks represent what performance you would get with a zero-overhead database implementation.

We do not include benchmarks against databases (e.g. MySQL or PostgreSQL) as the performance greatly differs between
deployments (e.g. request latency, database configuration) and tweaking individual things may greatly improve performance.
We believe, for that reason, that benchmark results for these database adapters are difficult to generalize and potentially
deceiving. They are thus not included.

This file is updated on every push to master. It thus represents the benchmark data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using flows such as the OAuth 2.0 Client Credentials
Grant, ORY Hydra validates the client credentials using BCrypt which causes (by design) CPU load. CPU load and performance
depend on the BCrypt cost which can be set using the environment variable `BCRYPT_COST`. For these benchmarks,
we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	1.3045 secs
  Slowest:	0.0876 secs
  Fastest:	0.0003 secs
  Average:	0.0125 secs
  Requests/sec:	7665.9140
  
  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.009 [4231]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.018 [3602]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.026 [1329]	|■■■■■■■■■■■■■
  0.035 [545]	|■■■■■
  0.044 [175]	|■■
  0.053 [51]	|
  0.061 [27]	|
  0.070 [19]	|
  0.079 [9]	|
  0.088 [11]	|


Latency distribution:
  10% in 0.0015 secs
  25% in 0.0049 secs
  50% in 0.0111 secs
  75% in 0.0168 secs
  90% in 0.0246 secs
  95% in 0.0311 secs
  99% in 0.0466 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0003 secs, 0.0876 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0095 secs
  req write:	0.0002 secs, 0.0000 secs, 0.0221 secs
  resp wait:	0.0117 secs, 0.0002 secs, 0.0868 secs
  resp read:	0.0004 secs, 0.0000 secs, 0.0123 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	24.5749 secs
  Slowest:	0.7678 secs
  Fastest:	0.0209 secs
  Average:	0.2391 secs
  Requests/sec:	406.9188
  
  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.021 [1]	|
  0.096 [536]	|■■■■■■
  0.170 [1633]	|■■■■■■■■■■■■■■■■■
  0.245 [3827]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.320 [2247]	|■■■■■■■■■■■■■■■■■■■■■■■
  0.394 [883]	|■■■■■■■■■
  0.469 [495]	|■■■■■
  0.544 [261]	|■■■
  0.618 [74]	|■
  0.693 [40]	|
  0.768 [3]	|


Latency distribution:
  10% in 0.1131 secs
  25% in 0.1808 secs
  50% in 0.2143 secs
  75% in 0.2978 secs
  90% in 0.3875 secs
  95% in 0.4256 secs
  99% in 0.5633 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0209 secs, 0.7678 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0142 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0769 secs
  resp wait:	0.2385 secs, 0.0208 secs, 0.7677 secs
  resp read:	0.0003 secs, 0.0000 secs, 0.0654 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading from  which negatively impacts
performance. Performance will be better if IDs and secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.7274 secs
  Slowest:	0.0375 secs
  Fastest:	0.0002 secs
  Average:	0.0070 secs
  Requests/sec:	13747.0789
  
  Total data:	4030000 bytes
  Size/request:	403 bytes

Response time histogram:
  0.000 [1]	|
  0.004 [2777]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.008 [2943]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.011 [2813]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.015 [1070]	|■■■■■■■■■■■■■■■
  0.019 [220]	|■■■
  0.023 [101]	|■
  0.026 [44]	|■
  0.030 [17]	|
  0.034 [12]	|
  0.037 [2]	|


Latency distribution:
  10% in 0.0007 secs
  25% in 0.0033 secs
  50% in 0.0067 secs
  75% in 0.0100 secs
  90% in 0.0120 secs
  95% in 0.0141 secs
  99% in 0.0216 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0002 secs, 0.0375 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0068 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0187 secs
  resp wait:	0.0046 secs, 0.0001 secs, 0.0248 secs
  resp read:	0.0012 secs, 0.0000 secs, 0.0141 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.5673 secs
  Slowest:	0.0293 secs
  Fastest:	0.0002 secs
  Average:	0.0054 secs
  Requests/sec:	17626.8327
  
  Total data:	4010000 bytes
  Size/request:	401 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [2947]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.006 [3644]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.009 [2096]	|■■■■■■■■■■■■■■■■■■■■■■■
  0.012 [806]	|■■■■■■■■■
  0.015 [269]	|■■■
  0.018 [129]	|■
  0.021 [65]	|■
  0.023 [8]	|
  0.026 [19]	|
  0.029 [16]	|


Latency distribution:
  10% in 0.0008 secs
  25% in 0.0028 secs
  50% in 0.0053 secs
  75% in 0.0071 secs
  90% in 0.0095 secs
  95% in 0.0119 secs
  99% in 0.0179 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0002 secs, 0.0293 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0102 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0121 secs
  resp wait:	0.0013 secs, 0.0001 secs, 0.0179 secs
  resp read:	0.0022 secs, 0.0000 secs, 0.0142 secs

Status code distribution:
  [200]	10000 responses



```