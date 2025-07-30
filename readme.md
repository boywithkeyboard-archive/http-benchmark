## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73968` | `2095` | `79995` |
| **86%** | [Hyper Express](#hyper-express) | `63900` | `5857` | `78122` |
| **35%** | [Node (Default)](#node-default) | `25821` | `7650` | `59169` |
| **32%** | [Fastify](#fastify) | `23501` | `6619` | `36399` |
| **28%** | [Hono](#hono) | `20686` | `5727` | `30260` |
| **25%** | [Koa](#koa) | `18719` | `6650` | `53139` |
| **11%** | [Carbon](#carbon) | `8137` | `1431` | `10424` |
| **8%** | [Express](#express) | `6130` | `990` | `8330` |


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
    Reqs/sec      8748.98    5866.28   73700.87
    Latency        5.70ms     4.28ms   370.52ms
    HTTP codes:
      1xx - 0, 2xx - 91681, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8319
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8319
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
    Reqs/sec      6284.23    1075.69    8408.77
    Latency        7.95ms     3.87ms   371.05ms
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
    Reqs/sec     23875.75    7181.52   35758.56
    Latency        2.09ms     2.08ms   184.62ms
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
    Reqs/sec     21720.45    6235.74   31362.77
    Latency        2.30ms     2.07ms   185.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     62219.64    3462.76   65151.77
    Latency      800.99us    77.15us     3.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18565.92    7519.18   67407.44
    Latency        2.69ms     2.40ms   212.28ms
    HTTP codes:
      1xx - 0, 2xx - 93161, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6839
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6839
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
    Reqs/sec     25987.43    8553.59   79245.89
    Latency        1.92ms     1.98ms   167.94ms
    HTTP codes:
      1xx - 0, 2xx - 96535, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3465
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3465
    Throughput:     5.75MB/s
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
    Reqs/sec     73871.91    2683.55   80040.03
    Latency      673.40us   190.48us    11.34ms
    HTTP codes:
      1xx - 0, 2xx - 95475, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4525
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4525
    Throughput:    11.17MB/s
  ```


