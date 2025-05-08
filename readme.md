## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72408` | `4443` | `81075` |
| **83%** | [Hyper Express](#hyper-express) | `60144` | `3343` | `65201` |
| **36%** | [Node (Default)](#node-default) | `25773` | `7622` | `53264` |
| **32%** | [Fastify](#fastify) | `23369` | `6803` | `35166` |
| **28%** | [Hono](#hono) | `20202` | `5464` | `29505` |
| **26%** | [Koa](#koa) | `19145` | `6840` | `52450` |
| **11%** | [Carbon](#carbon) | `8115` | `1373` | `10226` |
| **9%** | [Express](#express) | `6329` | `1038` | `8291` |


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
    Reqs/sec      8670.32    4721.70   53309.13
    Latency        5.76ms     4.29ms   372.16ms
    HTTP codes:
      1xx - 0, 2xx - 92528, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7472
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7472
    Throughput:     1.82MB/s
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
    Reqs/sec      6382.67    1058.32    8336.14
    Latency        7.83ms     3.68ms   355.51ms
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
    Reqs/sec     23857.64    7087.78   35836.32
    Latency        2.09ms     2.04ms   185.59ms
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
    Reqs/sec     20718.03    5927.70   29400.61
    Latency        2.41ms     2.02ms   185.12ms
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
    Reqs/sec     61583.40    3747.44   68513.43
    Latency      809.79us    75.28us     3.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     18799.76    7114.50   59445.96
    Latency        2.65ms     2.39ms   212.15ms
    HTTP codes:
      1xx - 0, 2xx - 93839, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6161
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6161
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
    Reqs/sec     25434.49    7044.50   55988.62
    Latency        1.96ms     1.86ms   160.04ms
    HTTP codes:
      1xx - 0, 2xx - 96594, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3406
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3406
    Throughput:     5.63MB/s
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
    Reqs/sec     72166.27    4668.85   83359.63
    Latency      691.00us   146.48us     5.04ms
    HTTP codes:
      1xx - 0, 2xx - 97509, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2491
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2491
    Throughput:    11.13MB/s
  ```


