## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72906` | `3045` | `85607` |
| **82%** | [Hyper Express](#hyper-express) | `60029` | `3151` | `63756` |
| **32%** | [Fastify](#fastify) | `23436` | `7966` | `37344` |
| **31%** | [Node (Default)](#node-default) | `22623` | `6938` | `74345` |
| **27%** | [Hono](#hono) | `19544` | `5969` | `29023` |
| **25%** | [Koa](#koa) | `18055` | `9148` | `78791` |
| **11%** | [Carbon](#carbon) | `8024` | `1518` | `10187` |
| **8%** | [Express](#express) | `6174` | `1070` | `8281` |


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
    Reqs/sec      8865.55    7941.16   79608.38
    Latency        5.63ms     4.51ms   385.99ms
    HTTP codes:
      1xx - 0, 2xx - 87010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12990
    Throughput:     1.75MB/s
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
    Reqs/sec      6314.14    1098.35    8301.39
    Latency        7.92ms     3.76ms   358.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24312.72    8413.43   38270.67
    Latency        2.06ms     2.14ms   192.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     20229.03    5906.04   30003.14
    Latency        2.47ms     2.10ms   188.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.57MB/s
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
    Reqs/sec     60292.90    3426.36   62650.72
    Latency      826.86us    78.79us     3.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.57MB/s
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
    Reqs/sec     18263.07    9185.45   79083.27
    Latency        2.73ms     2.39ms   211.88ms
    HTTP codes:
      1xx - 0, 2xx - 91142, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8858
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8858
    Throughput:     3.77MB/s
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
    Reqs/sec     22869.41    7042.16   74239.37
    Latency        2.18ms     1.93ms   167.71ms
    HTTP codes:
      1xx - 0, 2xx - 96252, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3748
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3748
    Throughput:     5.04MB/s
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
    Reqs/sec     73233.36    2844.45   80967.83
    Latency      680.27us   128.01us     5.15ms
    HTTP codes:
      1xx - 0, 2xx - 97615, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2385
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2385
    Throughput:    11.31MB/s
  ```


