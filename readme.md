## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74250` | `5261` | `87977` |
| **82%** | [Hyper Express](#hyper-express) | `60551` | `4577` | `67556` |
| **34%** | [Node (Default)](#node-default) | `25433` | `7036` | `52412` |
| **33%** | [Fastify](#fastify) | `24460` | `7832` | `36486` |
| **29%** | [Hono](#hono) | `21590` | `6071` | `30052` |
| **24%** | [Koa](#koa) | `17560` | `6351` | `51655` |
| **11%** | [Carbon](#carbon) | `8263` | `1395` | `10265` |
| **9%** | [Express](#express) | `6372` | `1018` | `8433` |


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
    Reqs/sec      8580.24    4197.71   53246.75
    Latency        5.82ms     4.30ms   371.37ms
    HTTP codes:
      1xx - 0, 2xx - 93812, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6188
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6188
    Throughput:     1.83MB/s
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
    Reqs/sec      6402.55    1015.79    8368.19
    Latency        7.81ms     3.62ms   347.83ms
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
    Reqs/sec     23076.48    6499.67   35768.76
    Latency        2.16ms     2.05ms   182.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.24MB/s
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
    Reqs/sec     20132.24    5681.64   29187.59
    Latency        2.48ms     2.08ms   189.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.55MB/s
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
    Reqs/sec     61888.65    3082.40   68106.24
    Latency      805.72us    68.38us     3.06ms
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
    Reqs/sec     19442.25    7459.14   53804.40
    Latency        2.57ms     2.35ms   209.61ms
    HTTP codes:
      1xx - 0, 2xx - 91216, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8784
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8784
    Throughput:     4.01MB/s
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
    Reqs/sec     25623.54    7590.36   56804.32
    Latency        1.95ms     1.94ms   167.45ms
    HTTP codes:
      1xx - 0, 2xx - 96823, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3177
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3177
    Throughput:     5.68MB/s
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
    Reqs/sec     72935.06    5038.54   80463.00
    Latency      683.42us   158.88us     7.40ms
    HTTP codes:
      1xx - 0, 2xx - 97984, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2016
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2016
    Throughput:    11.30MB/s
  ```


