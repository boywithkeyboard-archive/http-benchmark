## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75426` | `3703` | `89640` |
| **81%** | [Hyper Express](#hyper-express) | `61417` | `3309` | `64273` |
| **34%** | [Node (Default)](#node-default) | `25441` | `7932` | `60908` |
| **31%** | [Fastify](#fastify) | `23117` | `7182` | `36409` |
| **28%** | [Hono](#hono) | `21115` | `6337` | `31350` |
| **25%** | [Koa](#koa) | `18729` | `9149` | `75516` |
| **11%** | [Carbon](#carbon) | `8026` | `1386` | `10268` |
| **8%** | [Express](#express) | `6266` | `1031` | `8273` |


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
    Reqs/sec      8945.65    6393.67   71849.10
    Latency        5.58ms     4.24ms   365.16ms
    HTTP codes:
      1xx - 0, 2xx - 90551, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9449
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9449
    Throughput:     1.84MB/s
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
    Reqs/sec      6918.47    5853.85   72634.92
    Latency        7.22ms     4.00ms   364.67ms
    HTTP codes:
      1xx - 0, 2xx - 90203, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9797
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9797
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
    Reqs/sec     23260.69    7260.73   36554.11
    Latency        2.15ms     2.07ms   186.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.28MB/s
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
    Reqs/sec     20742.23    5545.39   29505.95
    Latency        2.41ms     2.04ms   183.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     61428.06    3401.30   64885.21
    Latency      811.40us    73.30us     2.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.73MB/s
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
    Reqs/sec     18820.34    7839.02   67050.29
    Latency        2.65ms     2.31ms   203.47ms
    HTTP codes:
      1xx - 0, 2xx - 93059, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6941
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6941
    Throughput:     3.96MB/s
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
    Reqs/sec     24557.45    8688.12   75315.08
    Latency        2.03ms     2.06ms   175.16ms
    HTTP codes:
      1xx - 0, 2xx - 95581, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4419
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4419
    Throughput:     5.38MB/s
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
    Reqs/sec     73182.62    2472.93   82044.61
    Latency      679.02us   192.43us    12.29ms
    HTTP codes:
      1xx - 0, 2xx - 96187, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3813
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3813
    Throughput:    11.14MB/s
  ```


