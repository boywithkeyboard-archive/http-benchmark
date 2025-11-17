## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73764` | `1958` | `76898` |
| **84%** | [Hyper Express](#hyper-express) | `61914` | `3349` | `65950` |
| **35%** | [Node (Default)](#node-default) | `25472` | `8242` | `64115` |
| **32%** | [Fastify](#fastify) | `23278` | `7251` | `34533` |
| **28%** | [Hono](#hono) | `20889` | `5833` | `29135` |
| **25%** | [Koa](#koa) | `18371` | `8485` | `78511` |
| **11%** | [Carbon](#carbon) | `8156` | `1415` | `10270` |
| **9%** | [Express](#express) | `6388` | `1064` | `8340` |


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
    Reqs/sec      9060.28    6759.06   72297.46
    Latency        5.51ms     4.24ms   363.88ms
    HTTP codes:
      1xx - 0, 2xx - 89820, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10180
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10180
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
    Reqs/sec      6421.61    1080.09    8347.48
    Latency        7.78ms     3.67ms   352.82ms
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
    Reqs/sec     22698.96    6489.71   35017.68
    Latency        2.20ms     2.03ms   184.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.15MB/s
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
    Reqs/sec     20103.39    5701.15   28430.11
    Latency        2.48ms     2.11ms   190.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.54MB/s
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
    Reqs/sec     61870.92    3602.07   68993.30
    Latency      805.92us    70.38us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.79MB/s
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
    Reqs/sec     18589.00    8779.53   77555.67
    Latency        2.69ms     2.49ms   217.49ms
    HTTP codes:
      1xx - 0, 2xx - 91300, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8700
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8700
    Throughput:     3.83MB/s
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
    Reqs/sec     25529.60    7689.08   63766.16
    Latency        1.95ms     1.99ms   170.40ms
    HTTP codes:
      1xx - 0, 2xx - 97325, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2675
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2675
    Throughput:     5.69MB/s
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
    Reqs/sec     73406.28    2713.95   78207.92
    Latency      678.32us   152.61us     6.81ms
    HTTP codes:
      1xx - 0, 2xx - 96635, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3365
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3365
    Throughput:    11.23MB/s
  ```


