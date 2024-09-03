## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73763` | `5267` | `79295` |
| **85%** | [Hyper Express](#hyper-express) | `62360` | `4363` | `74000` |
| **36%** | [Node (Default)](#node-default) | `26231` | `8141` | `42099` |
| **33%** | [Fastify](#fastify) | `24603` | `7736` | `35630` |
| **29%** | [Hono](#hono) | `21241` | `6217` | `29916` |
| **23%** | [Koa](#koa) | `17258` | `5178` | `40340` |
| **11%** | [Carbon](#carbon) | `8108` | `1391` | `10336` |
| **9%** | [Express](#express) | `6272` | `983` | `8392` |


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
    Reqs/sec      8352.50    3096.46   40777.30
    Latency        5.98ms     4.27ms   370.67ms
    HTTP codes:
      1xx - 0, 2xx - 96087, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3913
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3913
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
    Reqs/sec      6433.50    1066.35    8467.16
    Latency        7.77ms     3.67ms   354.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24522.60    7419.20   35552.25
    Latency        2.04ms     2.05ms   185.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     21152.25    6090.52   30720.88
    Latency        2.36ms     2.05ms   183.27ms
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
    Reqs/sec     61355.48    2370.67   63778.36
    Latency      812.49us    76.51us     3.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     18341.26    7043.51   54493.13
    Latency        2.72ms     2.39ms   204.48ms
    HTTP codes:
      1xx - 0, 2xx - 91994, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8006
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8006
    Throughput:     3.82MB/s
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
    Reqs/sec     26040.95    7344.25   49406.87
    Latency        1.92ms     1.91ms   164.38ms
    HTTP codes:
      1xx - 0, 2xx - 97863, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2137
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2137
    Throughput:     5.82MB/s
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
    Reqs/sec     74901.69    6103.34   85112.55
    Latency      665.73us   203.21us    10.28ms
    HTTP codes:
      1xx - 0, 2xx - 98183, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1817
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1817
    Throughput:    11.62MB/s
  ```


