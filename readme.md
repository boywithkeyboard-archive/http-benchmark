## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74068` | `5799` | `79209` |
| **87%** | [Hyper Express](#hyper-express) | `64610` | `5118` | `85101` |
| **35%** | [Node (Default)](#node-default) | `25851` | `7387` | `50907` |
| **33%** | [Fastify](#fastify) | `24472` | `7431` | `35958` |
| **27%** | [Hono](#hono) | `20089` | `5326` | `28799` |
| **24%** | [Koa](#koa) | `18056` | `6353` | `52579` |
| **11%** | [Carbon](#carbon) | `8270` | `1403` | `10507` |
| **9%** | [Express](#express) | `6754` | `1112` | `8644` |


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
    Reqs/sec      8703.21    4072.98   66972.87
    Latency        5.74ms     4.18ms   367.90ms
    HTTP codes:
      1xx - 0, 2xx - 94690, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5310
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5310
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
    Reqs/sec      6693.50    1084.53    8544.41
    Latency        7.47ms     3.42ms   329.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.91MB/s
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
    Reqs/sec     23985.81    6747.19   35138.17
    Latency        2.08ms     1.99ms   177.50ms
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
    Reqs/sec     21309.64    5757.05   29262.94
    Latency        2.34ms     2.02ms   183.88ms
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
    Reqs/sec     63469.75    3323.92   76494.85
    Latency      785.34us    68.27us     4.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.02MB/s
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
    Reqs/sec     18189.99    6316.26   61775.26
    Latency        2.74ms     2.27ms   200.98ms
    HTTP codes:
      1xx - 0, 2xx - 94510, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5490
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5490
    Throughput:     3.89MB/s
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
    Reqs/sec     25639.99    7051.09   48972.32
    Latency        1.95ms     1.80ms   157.50ms
    HTTP codes:
      1xx - 0, 2xx - 97844, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2156
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2156
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
    Reqs/sec     73878.52    4376.85   77997.75
    Latency      668.41us   106.75us     8.62ms
    HTTP codes:
      1xx - 0, 2xx - 97551, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2449
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2449
    Throughput:    11.41MB/s
  ```


