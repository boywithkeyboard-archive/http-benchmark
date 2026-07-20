## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `67462` | `3865` | `76526` |
| **84%** | [Hyper Express](#hyper-express) | `56652` | `3552` | `60859` |
| **31%** | [Fastify](#fastify) | `20709` | `6058` | `36120` |
| **29%** | [Hono](#hono) | `19784` | `6388` | `29502` |
| **29%** | [Node (Default)](#node-default) | `19634` | `4748` | `61303` |
| **26%** | [Koa](#koa) | `17571` | `7726` | `61362` |
| **11%** | [Carbon](#carbon) | `7237` | `1323` | `10189` |
| **9%** | [Express](#express) | `5985` | `1131` | `8073` |


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
    Reqs/sec      7975.63    5650.25   65495.95
    Latency        6.26ms     4.81ms   404.62ms
    HTTP codes:
      1xx - 0, 2xx - 91017, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8983
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8983
    Throughput:     1.65MB/s
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
    Reqs/sec      5854.86    1067.21    8039.59
    Latency        8.54ms     4.09ms   392.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.67MB/s
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
    Reqs/sec     19378.51    4419.18   36350.91
    Latency        2.58ms     2.08ms   191.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.40MB/s
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
    Reqs/sec     21289.02    7071.77   29391.85
    Latency        2.35ms     2.53ms   219.65ms
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
    Reqs/sec     57121.17    2991.52   63985.68
    Latency        0.87ms    92.48us     3.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.11MB/s
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
    Reqs/sec     18554.95    9508.50   75302.13
    Latency        2.69ms     2.46ms   216.30ms
    HTTP codes:
      1xx - 0, 2xx - 89546, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10454
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10454
    Throughput:     3.76MB/s
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
    Reqs/sec     19334.69    6130.38   78659.49
    Latency        2.58ms     2.06ms   177.03ms
    HTTP codes:
      1xx - 0, 2xx - 95277, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4723
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4723
    Throughput:     4.22MB/s
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
    Reqs/sec     68766.84    5792.27  106360.31
    Latency      729.45us   154.46us     6.15ms
    HTTP codes:
      1xx - 0, 2xx - 97228, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2772
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2772
    Throughput:    10.51MB/s
  ```


