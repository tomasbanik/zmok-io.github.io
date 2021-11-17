# Performance Tuning

## HTTP vs HTTPS
HTTPS requires an initial handshake which can be very slow. The actual amount of data transferred as part of the handshake isn't huge (under 5 kB typically), but for very small requests, this can be quite a bit of overhead. However, once the handshake is done, a very fast form of symmetric encryption is used, so the overhead there is minimal.

ZMOK focuses primarily on speed, so HTTP is also available, but consider using HTTP to read and using HTTPS for the send calls.

If you need to call more requests per second, consider using the HTTP endpoints:

```sh
$ git clone https://github.com/zmok-io/versus.git
$ # Follow installation instructions
$ ethspam | versus --stop-after=100 --concurrency=200  "https://api.zmok.io/mainnet/XXX" "http://api.zmok.io/mainnet/XXX"
Endpoints:

0. "https://api.zmok.io/mainnet/XXX"

   Requests:   585.46 per second
   Timing:     0.3416s avg, 0.1869s min, 1.1439s max
               0.1695s standard deviation

   Percentiles:
     25% in 0.2844s
     50% in 0.3094s
     75% in 0.3300s
     90% in 0.3588s
     95% in 0.8189s
     99% in 1.1439s

   Errors: 0.00%

1. "http://api.zmok.io/mainnet/XXX"

   Requests:   1035.88 per second
   Timing:     0.1931s avg, 0.0749s min, 1.0564s max
               0.1632s standard deviation

   Percentiles:
     25% in 0.1143s
     50% in 0.1414s
     75% in 0.1909s
     90% in 0.2596s
     95% in 0.6236s
     99% in 1.0564s

   Errors: 0.00%

** Summary for 2 endpoints:
   Completed:  100 results with 200 total requests
   Timing:     267.341168ms request avg, 1.285858648s total run time
   Errors:     0 (0.00%)
   Mismatched: 0
```

## Fastest block possible
To get the latest block as a first consider using Websocket endpoints with subscribe method "newBlockHeaders".

Benchmark of different API providers on how fast they get a new block:

```sh
$ git clone https://github.com/zmok-io/block-speed-comparison.git
$ cd block-speed-comparison
$ npm install
$ npm start

Waiting ...
------------------------------------------------------
New block from ZMOK-FR-WS #12884576 -> WINNER
Waiting ...
Received block from ZMOK-FR #12884576, LAG time: 0.207 sec
Received block from ZMOK #12884576, LAG time: 0.277 sec
Received block from ZMOK-WS #12884576, LAG time: 0.369 sec
Waiting ...
Received block from INFURA #12884576, LAG time: 1.076 sec
Waiting ...
------------------------------------------------------
New block from ZMOK-FR-WS #12884577 -> WINNER
Received block from ZMOK-WS #12884577, LAG time: 0.121 sec
Waiting ...
Received block from ZMOK #12884577, LAG time: 0.353 sec
Received block from ZMOK-FR #12884577, LAG time: 0.369 sec
------------------------------------------------------
New block from ZMOK-FR-WS #12884578 -> WINNER
Received block from ZMOK-WS #12884578, LAG time: 0.06 sec
Waiting ...
Received block from ZMOK-FR #12884578, LAG time: 0.728 sec
Received block from ZMOK #12884578, LAG time: 0.728 sec
Received block from ALCHEMY-WS #12884576, LAG blocks: 2
Waiting ...
Received block from INFURA #12884578, LAG time: 1.741 sec
------------------------------------------------------
New block from ZMOK-FR-WS #12884579 -> WINNER
Received block from ZMOK-WS #12884579, LAG time: 0.143 sec
Waiting ...
Received block from ZMOK-FR #12884579, LAG time: 0.765 sec
Received block from ZMOK #12884579, LAG time: 0.767 sec
Waiting ...
```
