# Blockatlas January 2020 Report

Current most critical issues:

| Project  | Type | URL                                                          |
| -------- | ---- | ------------------------------------------------------------ |
| api      | Bug  | [LINK](https://github.com/trustwallet/blockatlas/issues/783) |
| market  | Bug  | [LINK](https://github.com/trustwallet/blockatlas/issues/782) |
| observer | Bug  | [LINK](https://github.com/trustwallet/blockatlas/issues/778) |
| api      | Bug  | [LINK](https://github.com/trustwallet/blockatlas/issues/750) |
| market      | Bug  | [LINK](https://github.com/trustwallet/blockatlas/issues/752) |
| all      | Feature  | Soon |

Current refactoring that was happening:
https://github.com/trustwallet/blockatlas/pull/770

Was made as a first step:
 - to make microservices infrastructure for scaling
 - to reduce the entry barrier for developers
 - to stabilize the project and prevent from having new bugs 
 - to maintain with the lowest costs 
 - to be consistent with [best practices](https://github.com/golang-standards/project-layout) and [code style](https://github.com/uber-go/guide)

Future plans:
- REST api must be separated for prices, for staking and transactions. 
- Separate CRON, DB Reading, DB Saving, Blockchain and http fetching, use microservices for it
- Each plaform / api must be runned concurrently / async from others, no perform service crash of crashes
- Using good CI/CD infrastructure to monitor each api handle for errors, load balancing 

## Load testing 
```
Handler: https://blockatlas-trust-staging.herokuapp.com/v1/market/charts?coin=60&time_start=1574483028&max_items=64&currency=USD
Duration: 1m
Timeout: 2s
FAILS %: 90
Total REQ: 1519
Good response: 178
RPS: 20+
 
Handler: https://blockatlas-trust-staging.herokuapp.com/v1/tezos/tz1WCd2jm4uSt4vntk4vSuUWoZQGhLcDuR9q
Duration: 3m
Timeout: 8s
FAILS %: 99
 
Handler: https://blockatlas-trust-staging.herokuapp.com/v1/bitcoin/xpub/zpub6ruK9k6YGm8BRHWvTiQcrEPnFkuRDJhR7mPYzV2LDvjpLa5CuGgrhCYVZjMGcLcFqv9b2WvsFtY2Gb3xq8NVq8qhk9veozrA2W9QaWtihrC
Duration: 3m
Timeout: 10s
FAILS %: 0
Total REQ: 10651
Good response: 10651
RPS: 60
 
Handler: https://blockatlas-trust-staging.herokuapp.com/v2/tron/staking/delegations/TPJYCz8ppZNyvw7pTwmjajcx4Kk1MmEUhD
Duration: 1m
Timeout: 2s
FAILS %: 1
Total REQ: 14318
Good response: 14315
RPS: 200+
 
Handler: https://blockatlas-trust-staging.herokuapp.com/v2/tron/staking/delegations/TPJYCz8ppZNyvw7pTwmjajcx4Kk1MmEUhD
Duration: 1m
Timeout: 2s
FAILS %: 10
Total REQ: 4618
Good response: 4233
RPS: 80+
```

## Architecture problems
