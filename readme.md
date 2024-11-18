## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75654` | `6052` | `81533` |
| **83%** | [Hyper Express](#hyper-express) | `63159` | `3357` | `67904` |
| **34%** | [Node (Default)](#node-default) | `26048` | `7357` | `43864` |
| **32%** | [Fastify](#fastify) | `24079` | `6463` | `35603` |
| **28%** | [Hono](#hono) | `21555` | `5991` | `29591` |
| **24%** | [Koa](#koa) | `17996` | `7132` | `58981` |
| **11%** | [Carbon](#carbon) | `8191` | `1373` | `10504` |
| **9%** | [Express](#express) | `6684` | `1082` | `8582` |


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
    Reqs/sec      8703.19    3762.71   50422.28
    Latency        5.73ms     4.19ms   364.02ms
    HTTP codes:
      1xx - 0, 2xx - 94878, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5122
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5122
    Throughput:     1.88MB/s
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
    Reqs/sec      6583.48    1044.79    8507.63
    Latency        7.59ms     3.55ms   339.56ms
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
    Reqs/sec     23970.41    7036.45   35807.23
    Latency        2.08ms     1.97ms   177.81ms
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
    Reqs/sec     20761.90    5505.50   28272.90
    Latency        2.41ms     2.03ms   181.78ms
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
    Reqs/sec     63274.07    4365.53   71743.70
    Latency      788.59us    89.57us     5.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     17711.58    5607.22   43473.47
    Latency        2.82ms     2.24ms   195.74ms
    HTTP codes:
      1xx - 0, 2xx - 95310, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4690
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4690
    Throughput:     3.81MB/s
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
    Reqs/sec     25535.14    7346.71   55411.24
    Latency        1.95ms     1.89ms   160.69ms
    HTTP codes:
      1xx - 0, 2xx - 96678, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3322
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3322
    Throughput:     5.66MB/s
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
    Reqs/sec     75446.13    5828.79   84203.87
    Latency      660.12us   220.20us    11.65ms
    HTTP codes:
      1xx - 0, 2xx - 97477, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2523
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2523
    Throughput:    11.64MB/s
  ```


