# New Design of Blockatlas
New architecture design from NK, MS, AP backend team

## Whole overview
![Image Platform](https://raw.githubusercontent.com/EnoRage/blockatlas-report-jan-2020/master/blockatlas.jpeg)


We will have consistent REST API for operations with:
- Market (any prices, charts, excange rates)
- Blockchain ( any balances, txs, blocks, operations with external nodes/explorers)

There will be scheduler that can perform operation each N period, scheduler operations will be separted by Read / Write:
- Make HTTP Request to API (Latest block, Market Details .. any external data) and WRITE it to the Redis (for each API - different Redis)
- Read data from Redis and perform some calculaions over it (check for some parameters)

The whole blockatlas infrastructure will be:
- Markets API
- Plaform API
- Writers (Updaters) of Market data, Latest block data
- Readers that will perform opeartion like post webhook if you find tx of user at the block

Each blockchain, market api will have separate REST, Redis, Readers and Writers

## Plaform 
![Image Platform](https://github.com/EnoRage/blockatlas-report-jan-2020/raw/master/platform.png)

Now Plaform API with methods like:
- staking / delegations
- tx history
- blocks
- balance
- collectibles / routes / tokens

Are called Plaform API and will have their own REST HTTP Client. Each plaform coin will have it's own container. 
There will be a second layer cache per each api handler that will reduce the response time for the same request. Second layer cache will be updated each time the new block will come to the blockchain. Some methods will have another caching rules.
Plaform REST API must have atomic API Responses (we cannot separate the response logic more than will be at Plaform). 




## Observer

Blockchain parser (Single instace per blockchain):
1. Get latest block from network
2. Parse block to the `blockatlas.Block` struct
3. Set block to the MQ

REST Observer (Multiple instaces per blockchain): 
1. Handle request to subscribe `address, webhook` to the transactions
2. Save this request to the Redis 

Transactions and Address parser (Multiple instaces per blockchain):
1. Get `blockatlas.Block` from MQ and get `[]Adresses, []Webhooks` from Redis 
2. Check `blockatlas.Block` transactions if they have `[]Adresses`
4. Send Webhook with Transaction Details
5. If Block is parsed, remove it from Redis 

Redis and MQ - single instace per blockchain. Can be multiple with Master-Slave | Master-Master scaling if needs. (No needs, enough perfomance)

## Markets

Prices Parser (Single instance per API):
1. Fetch Data
2. Save to Redis Master

Prices REST (Multiple instances per API):
1. Handle request
2. Find Data in Redis Slave
3. Return Data with response
