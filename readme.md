## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74819` | `5022` | `81803` |
| **84%** | [Hyper Express](#hyper-express) | `62532` | `2695` | `65159` |
| **34%** | [Fastify](#fastify) | `25406` | `7962` | `36742` |
| **34%** | [Node (Default)](#node-default) | `25150` | `7083` | `50015` |
| **28%** | [Hono](#hono) | `20862` | `5857` | `30808` |
| **25%** | [Koa](#koa) | `18510` | `6748` | `56784` |
| **11%** | [Carbon](#carbon) | `8192` | `1411` | `10369` |
| **9%** | [Express](#express) | `6670` | `1117` | `8616` |


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
    Reqs/sec      8994.17    5084.00   58399.27
    Latency        5.54ms     4.24ms   369.13ms
    HTTP codes:
      1xx - 0, 2xx - 92268, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7732
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7732
    Throughput:     1.89MB/s
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
    Reqs/sec      6825.76    4310.42   57739.46
    Latency        7.31ms     3.91ms   357.86ms
    HTTP codes:
      1xx - 0, 2xx - 92490, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7510
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7510
    Throughput:     1.81MB/s
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
    Reqs/sec     23530.17    6867.47   35646.67
    Latency        2.12ms     2.01ms   180.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.34MB/s
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
    Reqs/sec     21142.69    6038.00   29875.05
    Latency        2.36ms     2.04ms   183.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     63025.98    4000.95   67402.35
    Latency      790.30us    72.01us     3.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     18818.68    6397.24   58032.35
    Latency        2.65ms     2.38ms   207.64ms
    HTTP codes:
      1xx - 0, 2xx - 94816, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5184
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5184
    Throughput:     4.04MB/s
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
    Reqs/sec     26455.50    7571.87   47006.62
    Latency        1.89ms     1.87ms   162.32ms
    HTTP codes:
      1xx - 0, 2xx - 98098, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1902
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1902
    Throughput:     5.93MB/s
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
    Reqs/sec     74917.72    5413.79   83074.63
    Latency      664.90us   165.92us     7.84ms
    HTTP codes:
      1xx - 0, 2xx - 96967, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3033
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3033
    Throughput:    11.49MB/s
  ```


