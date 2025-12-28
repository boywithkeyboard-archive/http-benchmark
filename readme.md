## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73830` | `2812` | `76952` |
| **84%** | [Hyper Express](#hyper-express) | `61912` | `2643` | `64547` |
| **34%** | [Node (Default)](#node-default) | `25396` | `8210` | `68260` |
| **32%** | [Fastify](#fastify) | `23283` | `7394` | `35195` |
| **28%** | [Hono](#hono) | `20310` | `5766` | `29214` |
| **25%** | [Koa](#koa) | `18641` | `8579` | `73263` |
| **11%** | [Carbon](#carbon) | `8241` | `1474` | `10377` |
| **9%** | [Express](#express) | `6460` | `1082` | `8333` |


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
    Reqs/sec      8653.62    5890.65   77802.00
    Latency        5.76ms     4.32ms   371.19ms
    HTTP codes:
      1xx - 0, 2xx - 91569, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8431
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8431
    Throughput:     1.80MB/s
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
    Reqs/sec      6206.69     992.12    8313.02
    Latency        8.05ms     3.63ms   349.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     22560.49    6733.44   35160.14
    Latency        2.21ms     2.04ms   184.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.12MB/s
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
    Reqs/sec     20133.28    5738.95   29530.23
    Latency        2.48ms     2.01ms   180.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.55MB/s
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
    Reqs/sec     62290.59    3581.07   65291.62
    Latency      798.65us    68.39us     2.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     18080.25    8982.69   83913.38
    Latency        2.75ms     2.39ms   212.94ms
    HTTP codes:
      1xx - 0, 2xx - 91706, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8294
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8294
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
    Reqs/sec     25367.34    8147.21   63380.24
    Latency        1.97ms     1.99ms   172.63ms
    HTTP codes:
      1xx - 0, 2xx - 97430, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2570
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2570
    Throughput:     5.65MB/s
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
    Reqs/sec     74507.45    2917.58   83079.13
    Latency      668.54us   134.97us     8.53ms
    HTTP codes:
      1xx - 0, 2xx - 97523, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2477
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2477
    Throughput:    11.49MB/s
  ```


