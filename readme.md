## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73265` | `5054` | `81535` |
| **84%** | [Hyper Express](#hyper-express) | `61776` | `3361` | `65985` |
| **36%** | [Node (Default)](#node-default) | `26307` | `7657` | `49598` |
| **35%** | [Fastify](#fastify) | `25433` | `8054` | `36243` |
| **30%** | [Hono](#hono) | `21614` | `6355` | `30746` |
| **26%** | [Koa](#koa) | `18946` | `7059` | `55317` |
| **11%** | [Carbon](#carbon) | `8235` | `1437` | `10408` |
| **9%** | [Express](#express) | `6507` | `1078` | `8495` |


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
    Reqs/sec      8702.95    4985.87   61411.82
    Latency        5.74ms     4.39ms   376.55ms
    HTTP codes:
      1xx - 0, 2xx - 92571, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7429
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7429
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
    Reqs/sec      6477.10    1097.93    8445.82
    Latency        7.71ms     3.68ms   350.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     25255.53    7596.58   35927.47
    Latency        1.98ms     2.13ms   189.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.73MB/s
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
    Reqs/sec     21573.15    5759.10   29687.58
    Latency        2.32ms     2.16ms   192.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     62104.50    3567.14   65619.73
    Latency      802.63us    76.08us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18861.71    6821.28   53348.74
    Latency        2.64ms     2.40ms   206.92ms
    HTTP codes:
      1xx - 0, 2xx - 93519, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6481
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6481
    Throughput:     3.99MB/s
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
    Reqs/sec     27810.44    8068.34   53038.10
    Latency        1.79ms     1.94ms   163.52ms
    HTTP codes:
      1xx - 0, 2xx - 97346, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2654
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2654
    Throughput:     6.20MB/s
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
    Reqs/sec     73503.22    5344.06   81721.75
    Latency      677.43us   171.99us    10.51ms
    HTTP codes:
      1xx - 0, 2xx - 96121, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3879
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3879
    Throughput:    11.18MB/s
  ```


