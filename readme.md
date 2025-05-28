## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73861` | `5007` | `85817` |
| **84%** | [Hyper Express](#hyper-express) | `61721` | `3630` | `65208` |
| **35%** | [Node (Default)](#node-default) | `25564` | `7319` | `52892` |
| **32%** | [Fastify](#fastify) | `23870` | `7438` | `36751` |
| **27%** | [Hono](#hono) | `19901` | `5345` | `30051` |
| **25%** | [Koa](#koa) | `18592` | `7140` | `57731` |
| **11%** | [Carbon](#carbon) | `8101` | `1396` | `10324` |
| **9%** | [Express](#express) | `6433` | `1108` | `8538` |


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
    Reqs/sec      8998.73    5496.06   56487.50
    Latency        5.55ms     4.44ms   381.93ms
    HTTP codes:
      1xx - 0, 2xx - 90663, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9337
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9337
    Throughput:     1.85MB/s
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
    Reqs/sec      6794.93    4684.04   58362.90
    Latency        7.35ms     4.06ms   371.76ms
    HTTP codes:
      1xx - 0, 2xx - 91345, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8655
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8655
    Throughput:     1.78MB/s
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
    Reqs/sec     24415.08    7763.69   36768.54
    Latency        2.05ms     1.98ms   179.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.53MB/s
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
    Reqs/sec     20539.76    5818.87   29924.70
    Latency        2.43ms     2.10ms   189.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.64MB/s
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
    Reqs/sec     61722.07    3496.97   65621.40
    Latency      807.65us    79.23us     2.96ms
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
    Reqs/sec     19789.67    7195.24   54974.45
    Latency        2.52ms     2.39ms   210.06ms
    HTTP codes:
      1xx - 0, 2xx - 93540, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6460
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6460
    Throughput:     4.18MB/s
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
    Reqs/sec     25478.63    7306.75   50626.78
    Latency        1.96ms     1.92ms   167.95ms
    HTTP codes:
      1xx - 0, 2xx - 97433, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2567
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2567
    Throughput:     5.68MB/s
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
    Reqs/sec     73113.38    5730.21   82505.62
    Latency      681.00us   200.54us     6.65ms
    HTTP codes:
      1xx - 0, 2xx - 96790, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3210
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3210
    Throughput:    11.20MB/s
  ```


