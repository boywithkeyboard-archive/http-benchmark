## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74905` | `2182` | `80008` |
| **83%** | [Hyper Express](#hyper-express) | `62328` | `3762` | `64974` |
| **34%** | [Node (Default)](#node-default) | `25433` | `7972` | `72282` |
| **32%** | [Fastify](#fastify) | `24239` | `7471` | `36269` |
| **27%** | [Hono](#hono) | `20589` | `5373` | `29073` |
| **25%** | [Koa](#koa) | `18974` | `9673` | `77747` |
| **11%** | [Carbon](#carbon) | `8297` | `1484` | `10470` |
| **9%** | [Express](#express) | `6419` | `1057` | `8451` |


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
    Reqs/sec      8873.65    5642.33   64567.74
    Latency        5.62ms     4.28ms   370.50ms
    HTTP codes:
      1xx - 0, 2xx - 92441, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7559
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7559
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
    Reqs/sec      6550.12    1077.38    8432.08
    Latency        7.63ms     3.63ms   347.04ms
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
    Reqs/sec     23403.95    7008.75   35117.15
    Latency        2.13ms     1.99ms   175.71ms
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
    Reqs/sec     20980.63    5871.62   29854.80
    Latency        2.38ms     2.03ms   179.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     63253.57    3555.57   66356.04
    Latency      788.28us    76.28us     3.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     18553.10    8906.37   78056.85
    Latency        2.69ms     2.35ms   205.73ms
    HTTP codes:
      1xx - 0, 2xx - 91715, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8285
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8285
    Throughput:     3.85MB/s
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
    Reqs/sec     24087.79    7355.24   62597.88
    Latency        2.07ms     1.83ms   158.50ms
    HTTP codes:
      1xx - 0, 2xx - 97545, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2455
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2455
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
    Reqs/sec     74608.38    2701.51   80838.76
    Latency      667.74us   127.45us     6.89ms
    HTTP codes:
      1xx - 0, 2xx - 96986, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3014
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3014
    Throughput:    11.45MB/s
  ```


