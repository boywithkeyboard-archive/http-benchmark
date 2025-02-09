## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73676` | `4385` | `81301` |
| **87%** | [Hyper Express](#hyper-express) | `63975` | `7104` | `78758` |
| **35%** | [Node (Default)](#node-default) | `25635` | `7816` | `45013` |
| **33%** | [Fastify](#fastify) | `24617` | `7621` | `37772` |
| **28%** | [Hono](#hono) | `20943` | `5683` | `30603` |
| **26%** | [Koa](#koa) | `18854` | `7035` | `53994` |
| **11%** | [Carbon](#carbon) | `8361` | `1434` | `10319` |
| **9%** | [Express](#express) | `6406` | `1035` | `8372` |


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
    Reqs/sec      8528.59    4467.58   53258.97
    Latency        5.85ms     4.33ms   372.99ms
    HTTP codes:
      1xx - 0, 2xx - 93631, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6369
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6369
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
    Reqs/sec      6450.21    1042.30    8359.20
    Latency        7.75ms     3.67ms   347.76ms
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
    Reqs/sec     24186.79    7289.16   36749.08
    Latency        2.07ms     2.06ms   184.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     21414.92    6539.41   30491.22
    Latency        2.33ms     2.03ms   179.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.84MB/s
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
    Reqs/sec     62904.07    3135.01   68068.73
    Latency      793.04us    70.92us     3.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     19117.70    7283.05   54438.63
    Latency        2.61ms     2.35ms   206.15ms
    HTTP codes:
      1xx - 0, 2xx - 92786, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7214
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7214
    Throughput:     4.01MB/s
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
    Reqs/sec     26344.65    7678.93   46859.63
    Latency        1.90ms     1.89ms   163.30ms
    HTTP codes:
      1xx - 0, 2xx - 98060, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1940
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1940
    Throughput:     5.90MB/s
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
    Reqs/sec     74462.32    5484.65   88625.34
    Latency      668.95us   200.83us    10.48ms
    HTTP codes:
      1xx - 0, 2xx - 96709, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3291
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3291
    Throughput:    11.38MB/s
  ```


