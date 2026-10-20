## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `101713` | `3586` | `110977` |
| **86%** | [Hyper Express](#hyper-express) | `87237` | `4282` | `90548` |
| **46%** | [Node (Default)](#node-default) | `46337` | `15960` | `89209` |
| **43%** | [Fastify](#fastify) | `43799` | `13886` | `64611` |
| **42%** | [Hono](#hono) | `42212` | `15137` | `57201` |
| **39%** | [Koa](#koa) | `39943` | `16819` | `103857` |
| **11%** | [Carbon](#carbon) | `11099` | `2391` | `16535` |
| **9%** | [Express](#express) | `9160` | `1953` | `13261` |


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
    Reqs/sec     12284.42    7048.37   81123.28
    Latency        4.05ms     3.52ms   300.55ms
    HTTP codes:
      1xx - 0, 2xx - 92994, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7006
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7006
    Throughput:     2.60MB/s
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
    Reqs/sec      9916.71    7705.31   91198.13
    Latency        5.03ms     3.09ms   283.38ms
    HTTP codes:
      1xx - 0, 2xx - 90507, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9493
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9493
    Throughput:     2.57MB/s
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
    Reqs/sec     44901.45   14333.61   63987.06
    Latency        1.11ms     1.41ms   124.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.18MB/s
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
    Reqs/sec     40847.01   15087.83   57395.94
    Latency        1.22ms     1.62ms   142.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.22MB/s
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
    Reqs/sec     87896.42    4880.82  107524.88
    Latency      569.42us    53.96us     3.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.44MB/s
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
    Reqs/sec     40179.58   14769.66   88704.48
    Latency        1.24ms     1.85ms   157.83ms
    HTTP codes:
      1xx - 0, 2xx - 92091, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7909
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7909
    Throughput:     8.37MB/s
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
    Reqs/sec     48437.61   15081.48   84217.25
    Latency        1.03ms     1.37ms   115.05ms
    HTTP codes:
      1xx - 0, 2xx - 96942, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3058
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3058
    Throughput:    10.74MB/s
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
    Reqs/sec    100936.51    3419.90  107024.28
    Latency      493.18us   164.15us     9.52ms
    HTTP codes:
      1xx - 0, 2xx - 93772, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6228
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6228
    Throughput:    14.99MB/s
  ```


