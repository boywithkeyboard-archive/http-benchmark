## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75836` | `6351` | `90770` |
| **84%** | [Hyper Express](#hyper-express) | `63429` | `4613` | `80516` |
| **33%** | [Node (Default)](#node-default) | `25138` | `6972` | `42570` |
| **31%** | [Fastify](#fastify) | `23374` | `7232` | `35479` |
| **27%** | [Hono](#hono) | `20409` | `5788` | `29648` |
| **24%** | [Koa](#koa) | `18264` | `5719` | `42667` |
| **11%** | [Carbon](#carbon) | `8413` | `1467` | `10384` |
| **8%** | [Express](#express) | `6298` | `932` | `8371` |


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
    Reqs/sec      8762.82    3377.99   49534.96
    Latency        5.69ms     4.16ms   359.99ms
    HTTP codes:
      1xx - 0, 2xx - 95484, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4516
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4516
    Throughput:     1.90MB/s
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
    Reqs/sec      6554.46    1021.18    8482.08
    Latency        7.62ms     3.56ms   341.83ms
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
    Reqs/sec     24053.00    7377.52   37082.91
    Latency        2.08ms     1.95ms   177.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     20765.34    5812.32   29088.54
    Latency        2.40ms     1.92ms   174.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     64032.51    3059.68   70691.55
    Latency      778.85us    72.65us     3.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.09MB/s
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
    Reqs/sec     18412.49    7412.04   65642.56
    Latency        2.71ms     2.40ms   209.03ms
    HTTP codes:
      1xx - 0, 2xx - 91870, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8130
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8130
    Throughput:     3.83MB/s
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
    Reqs/sec     26511.96    7906.42   43161.56
    Latency        1.88ms     1.82ms   157.56ms
    HTTP codes:
      1xx - 0, 2xx - 98255, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1745
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1745
    Throughput:     5.96MB/s
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
    Reqs/sec     75234.85    5840.40   85933.06
    Latency      661.63us   187.16us     9.80ms
    HTTP codes:
      1xx - 0, 2xx - 96834, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3166
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3166
    Throughput:    11.53MB/s
  ```


