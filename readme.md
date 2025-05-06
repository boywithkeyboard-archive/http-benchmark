## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72711` | `4216` | `79586` |
| **85%** | [Hyper Express](#hyper-express) | `62055` | `3413` | `66222` |
| **37%** | [Node (Default)](#node-default) | `26985` | `8092` | `54615` |
| **35%** | [Fastify](#fastify) | `25162` | `7672` | `38111` |
| **29%** | [Hono](#hono) | `21307` | `5824` | `29785` |
| **27%** | [Koa](#koa) | `19535` | `7122` | `55629` |
| **11%** | [Carbon](#carbon) | `8261` | `1454` | `10294` |
| **9%** | [Express](#express) | `6597` | `1115` | `8444` |


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
    Reqs/sec      8462.61    3707.98   49662.30
    Latency        5.90ms     4.28ms   369.30ms
    HTTP codes:
      1xx - 0, 2xx - 94624, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5376
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5376
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
    Reqs/sec      6485.87    1043.45    8352.47
    Latency        7.70ms     3.70ms   354.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     23587.02    6780.20   35961.69
    Latency        2.12ms     2.02ms   180.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.35MB/s
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
    Reqs/sec     20542.41    5541.85   29455.08
    Latency        2.43ms     2.07ms   185.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.64MB/s
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
    Reqs/sec     62070.63    3333.46   65957.14
    Latency      803.37us    68.50us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18499.28    6719.07   51261.42
    Latency        2.70ms     2.36ms   208.46ms
    HTTP codes:
      1xx - 0, 2xx - 94262, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5738
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5738
    Throughput:     3.95MB/s
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
    Reqs/sec     25401.33    7093.29   43616.20
    Latency        1.96ms     1.95ms   167.10ms
    HTTP codes:
      1xx - 0, 2xx - 98242, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1758
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1758
    Throughput:     5.72MB/s
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
    Reqs/sec     72832.60    4682.16   86543.17
    Latency      683.55us   169.78us     7.42ms
    HTTP codes:
      1xx - 0, 2xx - 97749, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2251
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2251
    Throughput:    11.27MB/s
  ```


