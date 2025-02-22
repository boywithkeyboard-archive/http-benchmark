## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74372` | `5339` | `85912` |
| **85%** | [Hyper Express](#hyper-express) | `63005` | `3500` | `68054` |
| **37%** | [Node (Default)](#node-default) | `27798` | `7470` | `47179` |
| **32%** | [Fastify](#fastify) | `23829` | `7415` | `37723` |
| **29%** | [Hono](#hono) | `21439` | `5943` | `30914` |
| **27%** | [Koa](#koa) | `19940` | `7510` | `56441` |
| **11%** | [Carbon](#carbon) | `8361` | `1453` | `10536` |
| **9%** | [Express](#express) | `6598` | `1065` | `8505` |


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
    Reqs/sec      8931.68    4865.48   59175.56
    Latency        5.59ms     4.22ms   367.05ms
    HTTP codes:
      1xx - 0, 2xx - 93012, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6988
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6988
    Throughput:     1.89MB/s
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
    Reqs/sec      6477.40    1008.67    8445.69
    Latency        7.72ms     3.59ms   342.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     24649.10    7548.10   37846.36
    Latency        2.03ms     1.96ms   177.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     21709.83    6346.44   30727.17
    Latency        2.30ms     2.01ms   178.25ms
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
    Reqs/sec     63391.22    4190.82   69606.03
    Latency      786.42us    69.43us     2.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     18931.48    7292.07   60399.00
    Latency        2.64ms     2.34ms   201.78ms
    HTTP codes:
      1xx - 0, 2xx - 92825, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7175
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7175
    Throughput:     3.97MB/s
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
    Reqs/sec     26734.77    7631.97   54287.39
    Latency        1.87ms     1.91ms   161.59ms
    HTTP codes:
      1xx - 0, 2xx - 97866, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2134
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2134
    Throughput:     5.99MB/s
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
    Reqs/sec     75250.97    5506.30   83524.77
    Latency      661.17us   206.04us     9.44ms
    HTTP codes:
      1xx - 0, 2xx - 95870, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4130
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4130
    Throughput:    11.42MB/s
  ```


