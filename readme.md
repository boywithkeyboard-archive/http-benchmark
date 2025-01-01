## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74423` | `6457` | `93406` |
| **85%** | [Hyper Express](#hyper-express) | `63577` | `4600` | `83659` |
| **34%** | [Node (Default)](#node-default) | `25127` | `7430` | `49693` |
| **32%** | [Fastify](#fastify) | `23528` | `6315` | `35381` |
| **28%** | [Hono](#hono) | `20856` | `5493` | `30015` |
| **25%** | [Koa](#koa) | `18631` | `5807` | `55381` |
| **11%** | [Carbon](#carbon) | `8242` | `1378` | `10558` |
| **9%** | [Express](#express) | `6454` | `983` | `8533` |


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
    Reqs/sec      8631.71    3259.38   41769.72
    Latency        5.78ms     4.29ms   371.80ms
    HTTP codes:
      1xx - 0, 2xx - 95744, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4256
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4256
    Throughput:     1.88MB/s
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
    Reqs/sec      6534.42    1044.43    8548.44
    Latency        7.65ms     3.61ms   345.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     23155.51    6260.90   36195.11
    Latency        2.16ms     2.05ms   183.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.26MB/s
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
    Reqs/sec     20443.20    5296.53   30098.84
    Latency        2.45ms     1.92ms   176.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     64539.14    3208.87   78259.17
    Latency      772.35us    81.81us     4.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.17MB/s
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
    Reqs/sec     19426.31    6334.02   52807.62
    Latency        2.57ms     2.25ms   196.01ms
    HTTP codes:
      1xx - 0, 2xx - 94511, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5489
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5489
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
    Reqs/sec     25039.10    7339.22   38772.51
    Latency        1.99ms     1.76ms   151.27ms
    HTTP codes:
      1xx - 0, 2xx - 98780, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1220
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1220
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
    Reqs/sec     74693.67    5600.29   82797.61
    Latency      666.62us   235.45us    18.83ms
    HTTP codes:
      1xx - 0, 2xx - 97962, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2038
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2038
    Throughput:    11.58MB/s
  ```


