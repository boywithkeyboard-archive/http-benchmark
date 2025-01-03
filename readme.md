## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73366` | `5595` | `78627` |
| **86%** | [Hyper Express](#hyper-express) | `62911` | `3739` | `68808` |
| **35%** | [Node (Default)](#node-default) | `25730` | `7400` | `55748` |
| **33%** | [Fastify](#fastify) | `24298` | `7173` | `36183` |
| **28%** | [Hono](#hono) | `20616` | `5587` | `29713` |
| **25%** | [Koa](#koa) | `18656` | `5815` | `41717` |
| **11%** | [Carbon](#carbon) | `8323` | `1415` | `10717` |
| **9%** | [Express](#express) | `6469` | `1023` | `8542` |


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
    Reqs/sec      8580.09    3465.87   47043.89
    Latency        5.82ms     4.20ms   364.25ms
    HTTP codes:
      1xx - 0, 2xx - 95538, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4462
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4462
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
    Reqs/sec      6295.96     919.45    8471.32
    Latency        7.94ms     3.66ms   348.47ms
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
    Reqs/sec     23950.36    7163.85   34629.34
    Latency        2.08ms     2.01ms   179.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.44MB/s
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
    Reqs/sec     20287.68    5243.20   29650.01
    Latency        2.46ms     2.01ms   180.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     63570.18    3153.16   69633.22
    Latency      784.52us    82.85us     5.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.03MB/s
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
    Reqs/sec     18011.98    5795.96   47898.31
    Latency        2.77ms     2.31ms   198.25ms
    HTTP codes:
      1xx - 0, 2xx - 94985, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5015
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5015
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
    Reqs/sec     26942.83    7681.07   45468.04
    Latency        1.85ms     1.91ms   163.75ms
    HTTP codes:
      1xx - 0, 2xx - 97980, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2020
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2020
    Throughput:     6.05MB/s
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
    Reqs/sec     74985.52    7327.83  103563.50
    Latency      664.24us   176.41us     9.00ms
    HTTP codes:
      1xx - 0, 2xx - 96734, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3266
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3266
    Throughput:    11.41MB/s
  ```


