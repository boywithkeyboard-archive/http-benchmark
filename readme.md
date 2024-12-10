## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73303` | `6615` | `92266` |
| **89%** | [Hyper Express](#hyper-express) | `65020` | `3457` | `69863` |
| **36%** | [Node (Default)](#node-default) | `26300` | `8204` | `58631` |
| **32%** | [Fastify](#fastify) | `23315` | `7182` | `35492` |
| **27%** | [Hono](#hono) | `19845` | `5268` | `29054` |
| **25%** | [Koa](#koa) | `18442` | `6210` | `53111` |
| **11%** | [Carbon](#carbon) | `8137` | `1340` | `10534` |
| **9%** | [Express](#express) | `6421` | `1022` | `8405` |


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
    Reqs/sec      8603.19    3593.89   56569.42
    Latency        5.80ms     4.21ms   366.11ms
    HTTP codes:
      1xx - 0, 2xx - 95280, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4720
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4720
    Throughput:     1.86MB/s
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
    Reqs/sec      6556.15    1065.42    8475.75
    Latency        7.62ms     3.52ms   338.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     23408.37    6715.50   36175.63
    Latency        2.13ms     1.85ms   172.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.31MB/s
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
    Reqs/sec     22028.79    6260.24   30481.37
    Latency        2.27ms     1.99ms   178.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.98MB/s
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
    Reqs/sec     64993.84    4254.86   71815.06
    Latency      767.44us    69.12us     3.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.23MB/s
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
    Reqs/sec     18140.56    5698.01   46529.67
    Latency        2.75ms     2.31ms   211.19ms
    HTTP codes:
      1xx - 0, 2xx - 95190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4810
    Throughput:     3.91MB/s
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
    Reqs/sec     24449.69    6718.75   40895.23
    Latency        2.04ms     1.89ms   162.70ms
    HTTP codes:
      1xx - 0, 2xx - 98348, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1652
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1652
    Throughput:     5.50MB/s
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
    Reqs/sec     75492.77    6595.95   90549.44
    Latency      658.33us   175.46us    22.95ms
    HTTP codes:
      1xx - 0, 2xx - 97302, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2698
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2698
    Throughput:    11.65MB/s
  ```


