## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73460` | `4931` | `81489` |
| **83%** | [Hyper Express](#hyper-express) | `61082` | `3250` | `63655` |
| **35%** | [Node (Default)](#node-default) | `25390` | `7881` | `54168` |
| **33%** | [Fastify](#fastify) | `24164` | `7571` | `37100` |
| **28%** | [Hono](#hono) | `20259` | `6113` | `30298` |
| **25%** | [Koa](#koa) | `18332` | `6178` | `49703` |
| **11%** | [Carbon](#carbon) | `8204` | `1420` | `10366` |
| **9%** | [Express](#express) | `6459` | `1077` | `8472` |


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
    Reqs/sec      8663.21    4993.77   55081.93
    Latency        5.76ms     4.30ms   372.73ms
    HTTP codes:
      1xx - 0, 2xx - 91657, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8343
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8343
    Throughput:     1.80MB/s
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
    Reqs/sec      6496.54    1098.06    8451.66
    Latency        7.69ms     3.72ms   356.55ms
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
    Reqs/sec     24115.16    7426.50   37201.06
    Latency        2.07ms     2.01ms   181.17ms
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
    Reqs/sec     21266.06    6047.17   29413.19
    Latency        2.35ms     2.09ms   187.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     61782.98    4463.78   66162.89
    Latency      806.61us   114.16us     5.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.77MB/s
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
    Reqs/sec     18450.83    6930.24   52524.64
    Latency        2.70ms     2.38ms   207.54ms
    HTTP codes:
      1xx - 0, 2xx - 93196, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6804
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6804
    Throughput:     3.89MB/s
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
    Reqs/sec     25115.91    7078.42   49024.07
    Latency        1.99ms     1.90ms   165.46ms
    HTTP codes:
      1xx - 0, 2xx - 97740, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2260
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2260
    Throughput:     5.62MB/s
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
    Reqs/sec     72659.04    8240.23   81182.91
    Latency      682.83us   295.75us    18.29ms
    HTTP codes:
      1xx - 0, 2xx - 96527, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3473
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3473
    Throughput:    11.12MB/s
  ```


