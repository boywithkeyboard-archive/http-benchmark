## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74327` | `3352` | `82529` |
| **84%** | [Hyper Express](#hyper-express) | `62723` | `3569` | `66564` |
| **34%** | [Node (Default)](#node-default) | `25593` | `10014` | `81003` |
| **33%** | [Fastify](#fastify) | `24184` | `7418` | `35654` |
| **27%** | [Hono](#hono) | `19708` | `5478` | `29152` |
| **26%** | [Koa](#koa) | `19271` | `9374` | `80481` |
| **11%** | [Carbon](#carbon) | `8158` | `1438` | `10263` |
| **9%** | [Express](#express) | `6390` | `1073` | `8397` |


### In Detail

- #### Carbon
  [NPM](https://npmjs.com/@sinclair/carbon) | [GitHub](https://github.com/sinclairzx81/carbon)
  ```js
  import { listen } from '@sinclair/carbon/http'

  listen({
    hostname: '127.0.0.1',
    port: 3000
  }, () => {
    return new Response('Hello World', {
      status: 200,
      headers: {
        'content-type': 'text/plain'
      }
    })
  })
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec      8704.98    6159.22   79358.71
    Latency        5.74ms     4.26ms   368.76ms
    HTTP codes:
      1xx - 0, 2xx - 91356, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8644
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8644
    Throughput:     1.81MB/s
  ```

- #### Express
  [NPM](https://npmjs.com/express) | [GitHub](https://github.com/expressjs/express)
  ```js
  import express from 'express'

  const app = express()

  app.get('/', function (req, res) {
    res.send('Hello World')
  })

  app.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec      7088.82    5680.25   75082.29
    Latency        7.04ms     4.01ms   369.36ms
    HTTP codes:
      1xx - 0, 2xx - 90677, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9323
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9323
    Throughput:     1.84MB/s
  ```

- #### Fastify
  [NPM](https://npmjs.com/fastify) | [GitHub](https://github.com/fastify/fastify)
  ```js
  import fastify from 'fastify'

  const app = fastify({
    logger: false
  })

  app.get('/', (req, res) => {
    res.send('Hello World')
  })

  app.listen({ port: 3000 }, (err) => {
    if (err) throw err
  })
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     24163.05    7685.78   35462.38
    Latency        2.07ms     2.06ms   186.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
  ```

- #### Hono
  [NPM](https://npmjs.com/hono) | [GitHub](https://github.com/honojs/hono)
  ```js
  import { serve } from '@hono/node-server'
  import { Hono } from 'hono'

  const app = new Hono()

  app.get('/', (c) => c.text('Hello World'))

  serve(app)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     20675.80    5815.06   30052.36
    Latency        2.42ms     2.00ms   181.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
  ```

- #### Hyper Express
  [NPM](https://npmjs.com/hyper-express) | [GitHub](https://github.com/kartikk221/hyper-express)
  ```js
  import HyperExpress from 'hyper-express'

  const server = new HyperExpress.Server()

  server.get('/', (req, res) => {
    res.send('Hello World')
  })

  server.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     61705.79    3026.67   64520.72
    Latency      808.00us    65.11us     2.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.77MB/s
  ```

- #### Koa
  [NPM](https://npmjs.com/koa) | [GitHub](https://github.com/koajs/koa)
  ```js
  import Koa from 'koa'

  const app = new Koa()

  app.use(ctx => {
    ctx.body = 'Hello World'
  })

  app.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     18104.24    7894.43   70763.98
    Latency        2.76ms     2.34ms   208.16ms
    HTTP codes:
      1xx - 0, 2xx - 92832, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7168
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7168
    Throughput:     3.80MB/s
  ```

- #### Node (Default)
  [Website](https://nodejs.org/api/http.html)
  ```js
  import { createServer } from 'node:http'

  const server = createServer((req, res) => {
    res.writeHead(200, {
      'content-type': 'text/plain'
    })

    res.write('Hello World')

    res.end()
  })

  server.listen(3000, '127.0.0.1')
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     25239.77    8015.54   64024.43
    Latency        1.98ms     1.87ms   159.98ms
    HTTP codes:
      1xx - 0, 2xx - 97423, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2577
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2577
    Throughput:     5.62MB/s
  ```

- #### uWS
  [GitHub](https://github.com/uNetworking/uWebSockets.js)
  ```js
  import { App } from 'uWebSockets.js'

  const app = App()

  app.get('/', (res, req) => {
    res.end('Hello World')
  })

  app.listen(3000, () => {})
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     74298.80    2333.29   77568.73
    Latency      670.68us   140.63us     7.48ms
    HTTP codes:
      1xx - 0, 2xx - 97320, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2680
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2680
    Throughput:    11.44MB/s
  ```


