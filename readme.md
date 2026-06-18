## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69109` | `3976` | `80997` |
| **81%** | [Hyper Express](#hyper-express) | `56003` | `5308` | `70579` |
| **30%** | [Fastify](#fastify) | `20627` | `5220` | `36040` |
| **30%** | [Hono](#hono) | `20465` | `5545` | `30081` |
| **29%** | [Koa](#koa) | `19836` | `9118` | `66228` |
| **29%** | [Node (Default)](#node-default) | `19736` | `5482` | `63664` |
| **11%** | [Carbon](#carbon) | `7329` | `1320` | `10362` |
| **9%** | [Express](#express) | `6057` | `1062` | `8184` |


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
    Reqs/sec      8338.95    6505.63   65886.59
    Latency        5.98ms     4.44ms   380.90ms
    HTTP codes:
      1xx - 0, 2xx - 88930, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11070
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11070
    Throughput:     1.69MB/s
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
    Reqs/sec      6154.58    1050.88    8565.39
    Latency        8.12ms     3.81ms   363.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     19796.65    4685.86   36389.24
    Latency        2.53ms     2.19ms   197.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.48MB/s
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
    Reqs/sec     21399.91    6599.45   30431.86
    Latency        2.34ms     2.16ms   191.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     58298.10    3443.23   65579.80
    Latency        0.86ms    93.70us     3.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.28MB/s
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
    Reqs/sec     19604.63   10200.99   77929.62
    Latency        2.55ms     2.56ms   217.84ms
    HTTP codes:
      1xx - 0, 2xx - 90027, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9973
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9973
    Throughput:     3.99MB/s
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
    Reqs/sec     20453.79    5078.96   71531.96
    Latency        2.44ms     1.94ms   165.29ms
    HTTP codes:
      1xx - 0, 2xx - 97010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2990
    Throughput:     4.55MB/s
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
    Reqs/sec     70529.42    4939.49   86297.74
    Latency      708.62us   204.67us    12.35ms
    HTTP codes:
      1xx - 0, 2xx - 95971, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4029
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4029
    Throughput:    10.68MB/s
  ```


