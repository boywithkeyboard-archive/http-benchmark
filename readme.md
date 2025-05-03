## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72601` | `6373` | `83039` |
| **86%** | [Hyper Express](#hyper-express) | `62671` | `2470` | `65614` |
| **36%** | [Node (Default)](#node-default) | `26210` | `7844` | `54234` |
| **31%** | [Fastify](#fastify) | `22678` | `6298` | `35152` |
| **29%** | [Hono](#hono) | `21051` | `5717` | `29817` |
| **27%** | [Koa](#koa) | `19267` | `7130` | `56360` |
| **11%** | [Carbon](#carbon) | `8119` | `1351` | `10329` |
| **9%** | [Express](#express) | `6431` | `1022` | `8454` |


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
    Reqs/sec      8689.88    5066.12   54770.62
    Latency        5.74ms     4.40ms   380.38ms
    HTTP codes:
      1xx - 0, 2xx - 91743, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8257
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8257
    Throughput:     1.81MB/s
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
    Reqs/sec      7082.31    5170.74   62105.36
    Latency        7.05ms     4.03ms   361.63ms
    HTTP codes:
      1xx - 0, 2xx - 90335, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9665
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9665
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
    Reqs/sec     23621.70    7081.02   35008.09
    Latency        2.11ms     2.03ms   184.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.36MB/s
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
    Reqs/sec     20787.30    5605.23   29884.34
    Latency        2.40ms     2.03ms   182.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     62730.00    2915.25   65208.05
    Latency      794.67us    61.31us     2.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     19331.72    7246.33   57178.79
    Latency        2.58ms     2.35ms   207.17ms
    HTTP codes:
      1xx - 0, 2xx - 93146, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6854
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6854
    Throughput:     4.07MB/s
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
    Reqs/sec     26470.37    7925.84   49376.23
    Latency        1.88ms     1.93ms   166.38ms
    HTTP codes:
      1xx - 0, 2xx - 97900, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2100
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2100
    Throughput:     5.93MB/s
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
    Reqs/sec     73249.67    3870.14   78908.65
    Latency      679.58us   184.92us     9.43ms
    HTTP codes:
      1xx - 0, 2xx - 96264, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3736
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3736
    Throughput:    11.16MB/s
  ```


