## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73012` | `4606` | `79805` |
| **82%** | [Hyper Express](#hyper-express) | `59854` | `3595` | `63184` |
| **35%** | [Node (Default)](#node-default) | `25444` | `7417` | `50557` |
| **33%** | [Fastify](#fastify) | `24171` | `7677` | `34698` |
| **27%** | [Hono](#hono) | `20064` | `5938` | `29010` |
| **25%** | [Koa](#koa) | `18593` | `7486` | `61223` |
| **11%** | [Carbon](#carbon) | `8044` | `1428` | `10117` |
| **9%** | [Express](#express) | `6266` | `1017` | `8200` |


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
    Reqs/sec      8305.60    3956.74   52869.24
    Latency        6.01ms     4.45ms   385.34ms
    HTTP codes:
      1xx - 0, 2xx - 94093, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5907
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5907
    Throughput:     1.78MB/s
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
    Reqs/sec      6245.77     997.16    8125.23
    Latency        8.00ms     3.82ms   364.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     24075.54    7483.17   34707.15
    Latency        2.08ms     2.10ms   185.84ms
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
    Reqs/sec     20938.13    5654.43   28426.44
    Latency        2.39ms     2.04ms   184.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     61103.29    2981.79   63228.40
    Latency      816.11us    74.76us     2.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.68MB/s
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
    Reqs/sec     18740.48    7006.94   57816.24
    Latency        2.66ms     2.46ms   220.69ms
    HTTP codes:
      1xx - 0, 2xx - 93598, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6402
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6402
    Throughput:     3.97MB/s
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
    Reqs/sec     26557.33    7854.65   58255.28
    Latency        1.88ms     2.03ms   171.75ms
    HTTP codes:
      1xx - 0, 2xx - 96205, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3795
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3795
    Throughput:     5.84MB/s
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
    Reqs/sec     72659.43    4640.79   79641.36
    Latency      686.02us   173.01us     7.62ms
    HTTP codes:
      1xx - 0, 2xx - 97606, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2394
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2394
    Throughput:    11.22MB/s
  ```


