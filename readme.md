## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74952` | `4204` | `92542` |
| **83%** | [Hyper Express](#hyper-express) | `62039` | `3395` | `65185` |
| **34%** | [Node (Default)](#node-default) | `25851` | `7733` | `58213` |
| **32%** | [Fastify](#fastify) | `23911` | `7337` | `35622` |
| **29%** | [Hono](#hono) | `21422` | `6404` | `31153` |
| **25%** | [Koa](#koa) | `18810` | `7874` | `70542` |
| **11%** | [Carbon](#carbon) | `8221` | `1420` | `10346` |
| **8%** | [Express](#express) | `6291` | `1026` | `8267` |


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
    Reqs/sec      8883.13    4994.58   57655.15
    Latency        5.61ms     4.43ms   379.97ms
    HTTP codes:
      1xx - 0, 2xx - 92422, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7578
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7578
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
    Reqs/sec      6412.01    1060.59    8300.97
    Latency        7.79ms     3.74ms   356.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     24280.94    7500.81   35652.94
    Latency        2.06ms     2.07ms   187.74ms
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
    Reqs/sec     21030.24    5699.48   30695.38
    Latency        2.38ms     1.90ms   174.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     62659.29    3382.37   65371.51
    Latency      795.81us    74.30us     3.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     19320.58    8735.13   72908.92
    Latency        2.58ms     2.38ms   210.40ms
    HTTP codes:
      1xx - 0, 2xx - 91694, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8306
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8306
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
    Reqs/sec     26447.57    9269.09   83341.45
    Latency        1.89ms     1.91ms   163.72ms
    HTTP codes:
      1xx - 0, 2xx - 95854, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4146
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4146
    Throughput:     5.79MB/s
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
    Reqs/sec     75334.18    3001.12   85198.05
    Latency      661.37us   142.61us     7.53ms
    HTTP codes:
      1xx - 0, 2xx - 96807, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3193
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3193
    Throughput:    11.54MB/s
  ```


