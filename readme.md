## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `77097` | `3346` | `88678` |
| **86%** | [Hyper Express](#hyper-express) | `66060` | `4653` | `71913` |
| **35%** | [Node (Default)](#node-default) | `26978` | `8537` | `66457` |
| **32%** | [Fastify](#fastify) | `25045` | `7597` | `38235` |
| **28%** | [Hono](#hono) | `21234` | `5911` | `31544` |
| **27%** | [Koa](#koa) | `20654` | `9397` | `76062` |
| **11%** | [Carbon](#carbon) | `8637` | `1588` | `11320` |
| **9%** | [Express](#express) | `7027` | `1208` | `9133` |


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
    Reqs/sec      9383.19    5909.99   71318.71
    Latency        5.30ms     4.13ms   356.31ms
    HTTP codes:
      1xx - 0, 2xx - 91910, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8090
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8090
    Throughput:     1.97MB/s
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
    Reqs/sec      6584.23    1026.65    8953.63
    Latency        7.59ms     3.55ms   340.57ms
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
    Reqs/sec     24963.91    7309.76   39641.05
    Latency        2.00ms     1.87ms   168.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.67MB/s
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
    Reqs/sec     21356.11    5601.01   33246.49
    Latency        2.34ms     1.94ms   178.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     65789.39    3735.19   72787.07
    Latency      758.88us    64.66us     2.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.33MB/s
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
    Reqs/sec     19994.22    8440.52   70959.49
    Latency        2.49ms     2.29ms   200.56ms
    HTTP codes:
      1xx - 0, 2xx - 92989, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7011
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7011
    Throughput:     4.22MB/s
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
    Reqs/sec     27739.32    9382.13   81909.43
    Latency        1.80ms     1.78ms   151.71ms
    HTTP codes:
      1xx - 0, 2xx - 96112, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3888
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3888
    Throughput:     6.10MB/s
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
    Reqs/sec     77713.09    4172.32   91977.37
    Latency      641.59us   120.73us     9.08ms
    HTTP codes:
      1xx - 0, 2xx - 97313, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2687
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2687
    Throughput:    11.96MB/s
  ```


