## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72891` | `4968` | `75652` |
| **85%** | [Hyper Express](#hyper-express) | `62202` | `4295` | `69276` |
| **37%** | [Node (Default)](#node-default) | `26646` | `8293` | `60308` |
| **33%** | [Fastify](#fastify) | `24253` | `7221` | `35802` |
| **30%** | [Hono](#hono) | `21846` | `6049` | `29548` |
| **25%** | [Koa](#koa) | `18560` | `7059` | `63707` |
| **11%** | [Carbon](#carbon) | `8326` | `1436` | `10638` |
| **9%** | [Express](#express) | `6318` | `980` | `8434` |


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
    Reqs/sec      8543.39    3630.27   54863.43
    Latency        5.84ms     4.16ms   358.76ms
    HTTP codes:
      1xx - 0, 2xx - 95010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4990
    Throughput:     1.85MB/s
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
    Reqs/sec      6480.17    1076.60    8608.48
    Latency        7.71ms     3.67ms   354.01ms
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
    Reqs/sec     24790.69    7511.83   35169.70
    Latency        2.02ms     2.06ms   185.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     21200.77    5932.66   29030.28
    Latency        2.36ms     2.01ms   183.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     62851.30    4187.38   70067.02
    Latency      792.98us    81.18us     4.10ms
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
    Reqs/sec     17591.94    5807.69   46635.89
    Latency        2.83ms     2.44ms   210.07ms
    HTTP codes:
      1xx - 0, 2xx - 94958, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5042
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5042
    Throughput:     3.78MB/s
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
    Reqs/sec     24833.94    7056.92   56489.81
    Latency        2.01ms     1.90ms   162.61ms
    HTTP codes:
      1xx - 0, 2xx - 97004, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2996
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2996
    Throughput:     5.52MB/s
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
    Reqs/sec     74757.12    6273.20   84776.17
    Latency      666.74us   201.41us     8.14ms
    HTTP codes:
      1xx - 0, 2xx - 97923, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2077
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2077
    Throughput:    11.57MB/s
  ```


