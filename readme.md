## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72776` | `2644` | `75694` |
| **83%** | [Hyper Express](#hyper-express) | `60416` | `3706` | `64442` |
| **34%** | [Node (Default)](#node-default) | `24924` | `7640` | `75578` |
| **32%** | [Fastify](#fastify) | `23003` | `7136` | `35294` |
| **27%** | [Hono](#hono) | `19736` | `5417` | `29502` |
| **27%** | [Koa](#koa) | `19290` | `9149` | `78827` |
| **11%** | [Carbon](#carbon) | `8109` | `1421` | `10255` |
| **9%** | [Express](#express) | `6348` | `1075` | `8245` |


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
    Reqs/sec      8503.05    5404.01   65536.89
    Latency        5.86ms     4.33ms   376.22ms
    HTTP codes:
      1xx - 0, 2xx - 92588, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7412
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7412
    Throughput:     1.79MB/s
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
    Reqs/sec      6341.57    1037.83    8276.96
    Latency        7.88ms     3.71ms   358.21ms
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
    Reqs/sec     23164.67    7417.89   34841.28
    Latency        2.16ms     2.09ms   188.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.26MB/s
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
    Reqs/sec     20002.61    5592.22   29000.72
    Latency        2.50ms     2.05ms   183.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.52MB/s
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
    Reqs/sec     61312.61    3402.69   64618.78
    Latency      813.31us    76.57us     3.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     17540.50    7028.10   61348.86
    Latency        2.85ms     2.42ms   213.30ms
    HTTP codes:
      1xx - 0, 2xx - 93767, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6233
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6233
    Throughput:     3.72MB/s
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
    Reqs/sec     25830.56    8475.62   58631.37
    Latency        1.93ms     1.85ms   159.96ms
    HTTP codes:
      1xx - 0, 2xx - 97607, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2393
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2393
    Throughput:     5.78MB/s
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
    Reqs/sec     73994.26    2201.28   80748.75
    Latency      673.13us   145.95us     9.83ms
    HTTP codes:
      1xx - 0, 2xx - 95579, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4421
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4421
    Throughput:    11.19MB/s
  ```


