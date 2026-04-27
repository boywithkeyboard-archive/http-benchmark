## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71858` | `3783` | `82166` |
| **84%** | [Hyper Express](#hyper-express) | `60314` | `3905` | `66660` |
| **30%** | [Hono](#hono) | `21365` | `6940` | `31279` |
| **29%** | [Fastify](#fastify) | `21124` | `5936` | `37019` |
| **28%** | [Node (Default)](#node-default) | `20193` | `5897` | `67691` |
| **25%** | [Koa](#koa) | `17954` | `8325` | `65530` |
| **10%** | [Carbon](#carbon) | `7348` | `1202` | `10136` |
| **9%** | [Express](#express) | `6178` | `1136` | `8319` |


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
    Reqs/sec      8119.88    6342.18   74106.72
    Latency        6.15ms     4.46ms   382.55ms
    HTTP codes:
      1xx - 0, 2xx - 89478, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10522
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10522
    Throughput:     1.65MB/s
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
    Reqs/sec      6177.34    1109.20    8180.94
    Latency        8.09ms     3.94ms   375.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     21331.47    6031.95   37221.95
    Latency        2.34ms     2.21ms   199.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.84MB/s
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
    Reqs/sec     21469.77    6360.80   31020.80
    Latency        2.33ms     2.01ms   183.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     59401.10    3039.46   64694.56
    Latency      840.13us    83.30us     3.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.43MB/s
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
    Reqs/sec     19958.16    9307.28   69587.72
    Latency        2.50ms     2.59ms   224.68ms
    HTTP codes:
      1xx - 0, 2xx - 91839, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8161
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8161
    Throughput:     4.15MB/s
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
    Reqs/sec     21068.86    5254.21   60093.48
    Latency        2.37ms     2.07ms   175.69ms
    HTTP codes:
      1xx - 0, 2xx - 97291, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2709
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2709
    Throughput:     4.69MB/s
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
    Reqs/sec     73606.04    6194.95   87739.35
    Latency      676.97us   163.64us     9.99ms
    HTTP codes:
      1xx - 0, 2xx - 97280, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2720
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2720
    Throughput:    11.33MB/s
  ```


