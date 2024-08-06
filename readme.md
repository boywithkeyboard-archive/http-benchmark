## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72471` | `7540` | `79141` |
| **87%** | [Hyper Express](#hyper-express) | `63328` | `4093` | `75847` |
| **37%** | [Node (Default)](#node-default) | `26687` | `8584` | `57410` |
| **33%** | [Fastify](#fastify) | `23751` | `6726` | `36002` |
| **28%** | [Hono](#hono) | `20601` | `5757` | `29686` |
| **26%** | [Koa](#koa) | `18834` | `7035` | `56948` |
| **12%** | [Carbon](#carbon) | `8342` | `1443` | `10593` |
| **9%** | [Express](#express) | `6504` | `1050` | `8555` |


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
    Reqs/sec      8604.13    3852.09   58521.03
    Latency        5.80ms     4.12ms   354.69ms
    HTTP codes:
      1xx - 0, 2xx - 94891, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5109
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5109
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
    Reqs/sec      6518.79    1032.44    8464.88
    Latency        7.67ms     3.60ms   345.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     23926.74    7141.76   36737.56
    Latency        2.09ms     2.05ms   182.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     21041.74    6170.10   30486.40
    Latency        2.37ms     2.01ms   180.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     62658.12    2942.10   65942.96
    Latency      795.64us    62.69us     2.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     18167.27    6841.43   69746.39
    Latency        2.75ms     2.33ms   212.59ms
    HTTP codes:
      1xx - 0, 2xx - 93849, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6151
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6151
    Throughput:     3.86MB/s
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
    Reqs/sec     25741.50    7803.99   62240.65
    Latency        1.94ms     1.87ms   160.44ms
    HTTP codes:
      1xx - 0, 2xx - 96875, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3125
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3125
    Throughput:     5.71MB/s
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
    Reqs/sec     73613.32    5187.14   90231.38
    Latency      677.24us   148.04us    11.89ms
    HTTP codes:
      1xx - 0, 2xx - 97163, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2837
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2837
    Throughput:    11.31MB/s
  ```


