## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75248` | `2587` | `82688` |
| **84%** | [Hyper Express](#hyper-express) | `63020` | `3009` | `66626` |
| **33%** | [Node (Default)](#node-default) | `24961` | `7273` | `65693` |
| **32%** | [Fastify](#fastify) | `23835` | `6988` | `36128` |
| **28%** | [Hono](#hono) | `21050` | `5762` | `30613` |
| **25%** | [Koa](#koa) | `18857` | `8648` | `79212` |
| **11%** | [Carbon](#carbon) | `8176` | `1423` | `10344` |
| **8%** | [Express](#express) | `6185` | `961` | `8225` |


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
    Reqs/sec      8914.44    6684.17   77882.39
    Latency        5.60ms     4.44ms   381.29ms
    HTTP codes:
      1xx - 0, 2xx - 89902, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10098
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10098
    Throughput:     1.82MB/s
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
    Reqs/sec      6287.46    1006.45    8349.90
    Latency        7.95ms     3.66ms   351.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     24067.50    7268.75   35626.06
    Latency        2.08ms     2.01ms   180.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     21018.76    5484.48   29866.19
    Latency        2.38ms     1.94ms   174.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     63330.76    3228.75   66273.54
    Latency      787.32us    70.63us     2.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     18799.09    8218.55   77143.02
    Latency        2.65ms     2.41ms   214.60ms
    HTTP codes:
      1xx - 0, 2xx - 92711, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7289
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7289
    Throughput:     3.94MB/s
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
    Reqs/sec     26205.97    8541.23   77116.30
    Latency        1.90ms     1.96ms   170.48ms
    HTTP codes:
      1xx - 0, 2xx - 96466, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3534
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3534
    Throughput:     5.81MB/s
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
    Reqs/sec     75910.74    4091.15  101957.59
    Latency      659.42us   167.35us     8.62ms
    HTTP codes:
      1xx - 0, 2xx - 96254, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3746
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3746
    Throughput:    11.51MB/s
  ```


