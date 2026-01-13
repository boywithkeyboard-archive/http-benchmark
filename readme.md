## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72528` | `2281` | `81212` |
| **80%** | [Hyper Express](#hyper-express) | `57858` | `2574` | `60979` |
| **33%** | [Node (Default)](#node-default) | `23785` | `8078` | `72031` |
| **33%** | [Fastify](#fastify) | `23736` | `8144` | `37668` |
| **29%** | [Hono](#hono) | `20768` | `6770` | `31782` |
| **26%** | [Koa](#koa) | `18580` | `8399` | `66912` |
| **11%** | [Carbon](#carbon) | `7855` | `1462` | `10434` |
| **9%** | [Express](#express) | `6232` | `1083` | `8439` |


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
    Reqs/sec      8534.94    6396.29   75131.64
    Latency        5.85ms     4.34ms   372.68ms
    HTTP codes:
      1xx - 0, 2xx - 90462, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9538
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9538
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
    Reqs/sec      6272.48    1097.13    8296.65
    Latency        7.97ms     3.78ms   358.94ms
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
    Reqs/sec     22323.90    7147.54   38075.67
    Latency        2.24ms     2.11ms   189.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.06MB/s
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
    Reqs/sec     20049.86    6254.66   30707.26
    Latency        2.49ms     2.08ms   188.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.53MB/s
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
    Reqs/sec     59512.28    3359.39   62435.70
    Latency      837.91us    72.63us     2.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.45MB/s
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
    Reqs/sec     18393.56    8070.67   67070.81
    Latency        2.71ms     2.39ms   210.72ms
    HTTP codes:
      1xx - 0, 2xx - 92965, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7035
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7035
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
    Reqs/sec     24204.46    7794.41   65697.62
    Latency        2.06ms     1.91ms   165.55ms
    HTTP codes:
      1xx - 0, 2xx - 97323, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2677
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2677
    Throughput:     5.39MB/s
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
    Reqs/sec     72434.40    2592.93   78187.67
    Latency      687.88us   143.11us     6.91ms
    HTTP codes:
      1xx - 0, 2xx - 97373, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2627
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2627
    Throughput:    11.16MB/s
  ```


