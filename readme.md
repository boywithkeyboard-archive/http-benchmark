## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75488` | `3741` | `87277` |
| **82%** | [Hyper Express](#hyper-express) | `61929` | `3678` | `64633` |
| **33%** | [Node (Default)](#node-default) | `24858` | `7942` | `62027` |
| **32%** | [Fastify](#fastify) | `24293` | `7078` | `35789` |
| **28%** | [Hono](#hono) | `20870` | `5746` | `29706` |
| **26%** | [Koa](#koa) | `19518` | `8743` | `78043` |
| **11%** | [Carbon](#carbon) | `8456` | `1468` | `10507` |
| **9%** | [Express](#express) | `6662` | `1153` | `8514` |


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
    Reqs/sec      9373.21    7707.47   79732.82
    Latency        5.33ms     4.21ms   364.64ms
    HTTP codes:
      1xx - 0, 2xx - 88065, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11935
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11935
    Throughput:     1.87MB/s
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
    Reqs/sec      7004.76    5258.32   64275.83
    Latency        7.12ms     3.97ms   362.91ms
    HTTP codes:
      1xx - 0, 2xx - 91141, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8859
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8859
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
    Reqs/sec     24188.57    7321.17   35592.95
    Latency        2.07ms     2.09ms   185.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     20946.73    5695.09   29991.48
    Latency        2.39ms     1.94ms   175.14ms
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
    Reqs/sec     62845.04    3766.66   65590.36
    Latency      793.28us    73.06us     3.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     20159.05    9561.35   77519.55
    Latency        2.47ms     2.34ms   206.12ms
    HTTP codes:
      1xx - 0, 2xx - 90742, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9258
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9258
    Throughput:     4.14MB/s
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
    Reqs/sec     25467.97    7978.83   65003.71
    Latency        1.96ms     1.97ms   171.65ms
    HTTP codes:
      1xx - 0, 2xx - 97194, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2806
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2806
    Throughput:     5.67MB/s
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
    Reqs/sec     74185.36    2433.72   79836.15
    Latency      670.80us   165.04us    10.77ms
    HTTP codes:
      1xx - 0, 2xx - 96524, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3476
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3476
    Throughput:    11.33MB/s
  ```


