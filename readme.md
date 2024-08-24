## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74039` | `5425` | `82997` |
| **86%** | [Hyper Express](#hyper-express) | `63384` | `3005` | `79396` |
| **35%** | [Node (Default)](#node-default) | `26202` | `7751` | `56132` |
| **34%** | [Fastify](#fastify) | `24915` | `7779` | `36563` |
| **27%** | [Hono](#hono) | `20352` | `5645` | `29790` |
| **25%** | [Koa](#koa) | `18141` | `7456` | `62430` |
| **11%** | [Carbon](#carbon) | `8334` | `1421` | `10587` |
| **9%** | [Express](#express) | `6470` | `1004` | `8461` |


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
    Reqs/sec      8723.41    5287.25   64251.34
    Latency        5.72ms     4.19ms   362.61ms
    HTTP codes:
      1xx - 0, 2xx - 92031, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7969
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7969
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
    Reqs/sec      6606.58    1111.38    8528.92
    Latency        7.56ms     3.60ms   349.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     24634.42    8119.12   37420.73
    Latency        2.03ms     2.08ms   185.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.58MB/s
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
    Reqs/sec     21291.73    5970.44   30227.85
    Latency        2.35ms     2.03ms   184.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.81MB/s
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
    Reqs/sec     61785.40    2868.53   64147.49
    Latency      806.85us    70.05us     3.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.78MB/s
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
    Reqs/sec     16976.53    5653.59   43111.05
    Latency        2.94ms     2.38ms   209.73ms
    HTTP codes:
      1xx - 0, 2xx - 95375, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4625
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4625
    Throughput:     3.66MB/s
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
    Reqs/sec     26342.61    6954.87   51176.49
    Latency        1.89ms     1.88ms   159.77ms
    HTTP codes:
      1xx - 0, 2xx - 97384, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2616
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2616
    Throughput:     5.87MB/s
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
    Reqs/sec     74699.03    5565.70   87377.20
    Latency      667.05us   222.24us    11.09ms
    HTTP codes:
      1xx - 0, 2xx - 96951, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3049
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3049
    Throughput:    11.46MB/s
  ```


