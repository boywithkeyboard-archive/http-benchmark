## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74683` | `6373` | `87417` |
| **84%** | [Hyper Express](#hyper-express) | `62552` | `3422` | `68340` |
| **35%** | [Node (Default)](#node-default) | `25770` | `7683` | `43972` |
| **33%** | [Fastify](#fastify) | `24878` | `7820` | `35700` |
| **29%** | [Hono](#hono) | `21374` | `5497` | `30005` |
| **25%** | [Koa](#koa) | `18314` | `6588` | `58702` |
| **11%** | [Carbon](#carbon) | `8231` | `1447` | `10662` |
| **8%** | [Express](#express) | `6330` | `958` | `8333` |


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
    Reqs/sec      8510.93    3012.23   42665.08
    Latency        5.86ms     4.39ms   375.11ms
    HTTP codes:
      1xx - 0, 2xx - 96293, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3707
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3707
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
    Reqs/sec      6297.38     960.80    8393.67
    Latency        7.94ms     3.64ms   348.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     24112.37    7025.59   35864.66
    Latency        2.07ms     1.93ms   177.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     21522.00    6520.42   30102.33
    Latency        2.32ms     2.04ms   181.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     62480.07    4152.21   76387.75
    Latency      798.14us    73.13us     3.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     17670.11    6343.29   57165.50
    Latency        2.82ms     2.40ms   214.52ms
    HTTP codes:
      1xx - 0, 2xx - 94249, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5751
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5751
    Throughput:     3.77MB/s
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
    Reqs/sec     25741.09    7700.52   45014.66
    Latency        1.94ms     1.84ms   158.21ms
    HTTP codes:
      1xx - 0, 2xx - 98200, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1800
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1800
    Throughput:     5.79MB/s
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
    Reqs/sec     74048.56    5469.91   86321.80
    Latency      672.24us   232.63us    14.75ms
    HTTP codes:
      1xx - 0, 2xx - 96649, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3351
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3351
    Throughput:    11.32MB/s
  ```


