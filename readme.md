## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73209` | `5151` | `81794` |
| **84%** | [Hyper Express](#hyper-express) | `61433` | `4325` | `67146` |
| **34%** | [Fastify](#fastify) | `24890` | `7897` | `36255` |
| **34%** | [Node (Default)](#node-default) | `24853` | `7064` | `47582` |
| **30%** | [Hono](#hono) | `21840` | `6160` | `30611` |
| **25%** | [Koa](#koa) | `18102` | `6394` | `50588` |
| **11%** | [Carbon](#carbon) | `8226` | `1408` | `10484` |
| **9%** | [Express](#express) | `6463` | `1079` | `8533` |


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
    Reqs/sec      8663.72    3806.60   52016.59
    Latency        5.73ms     4.28ms   368.94ms
    HTTP codes:
      1xx - 0, 2xx - 94570, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5430
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5430
    Throughput:     1.87MB/s
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
    Reqs/sec      6339.38    1026.69    8462.72
    Latency        7.89ms     3.63ms   348.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     24283.23    7580.41   37508.32
    Latency        2.06ms     2.04ms   183.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     21479.11    6460.43   30648.85
    Latency        2.33ms     2.05ms   182.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     61869.19    3998.23   65010.11
    Latency      805.55us   105.90us     5.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.79MB/s
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
    Reqs/sec     18017.94    6178.56   46356.97
    Latency        2.77ms     2.45ms   211.39ms
    HTTP codes:
      1xx - 0, 2xx - 94868, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5132
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5132
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
    Reqs/sec     25450.17    7534.56   39715.56
    Latency        1.96ms     2.06ms   177.48ms
    HTTP codes:
      1xx - 0, 2xx - 98400, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1600
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1600
    Throughput:     5.74MB/s
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
    Reqs/sec     73793.56    6487.59   79377.80
    Latency      674.76us   238.27us    12.11ms
    HTTP codes:
      1xx - 0, 2xx - 96844, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3156
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3156
    Throughput:    11.31MB/s
  ```


