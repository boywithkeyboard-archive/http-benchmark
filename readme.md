## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72220` | `6428` | `89996` |
| **87%** | [Hyper Express](#hyper-express) | `62560` | `3766` | `65149` |
| **36%** | [Node (Default)](#node-default) | `26321` | `7072` | `45515` |
| **34%** | [Fastify](#fastify) | `24446` | `7437` | `35788` |
| **30%** | [Hono](#hono) | `21320` | `6307` | `30317` |
| **26%** | [Koa](#koa) | `18867` | `6929` | `56081` |
| **11%** | [Carbon](#carbon) | `8150` | `1329` | `10287` |
| **9%** | [Express](#express) | `6542` | `1105` | `8424` |


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
    Reqs/sec      8671.15    4990.26   56565.82
    Latency        5.75ms     4.28ms   369.29ms
    HTTP codes:
      1xx - 0, 2xx - 92321, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7679
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7679
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
    Reqs/sec      6406.24    1031.17    8312.83
    Latency        7.80ms     3.72ms   358.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     24217.92    7071.22   36870.76
    Latency        2.06ms     2.04ms   184.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     21207.75    6045.09   30153.11
    Latency        2.35ms     2.06ms   185.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     64211.71    3415.78   69533.79
    Latency      776.64us    70.62us     4.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.12MB/s
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
    Reqs/sec     18397.88    7136.78   61267.57
    Latency        2.71ms     2.33ms   205.25ms
    HTTP codes:
      1xx - 0, 2xx - 93053, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6947
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6947
    Throughput:     3.87MB/s
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
    Reqs/sec     25634.22    7860.02   63566.93
    Latency        1.94ms     1.91ms   162.03ms
    HTTP codes:
      1xx - 0, 2xx - 96392, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3608
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3608
    Throughput:     5.66MB/s
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
    Reqs/sec     72736.33    4867.67   79414.60
    Latency      684.89us   150.11us     6.46ms
    HTTP codes:
      1xx - 0, 2xx - 98117, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1883
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1883
    Throughput:    11.29MB/s
  ```


