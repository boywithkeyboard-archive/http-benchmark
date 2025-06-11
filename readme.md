## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72848` | `5500` | `79947` |
| **86%** | [Hyper Express](#hyper-express) | `62358` | `3852` | `67107` |
| **34%** | [Node (Default)](#node-default) | `24460` | `6873` | `49756` |
| **32%** | [Fastify](#fastify) | `23653` | `7495` | `36042` |
| **29%** | [Hono](#hono) | `20817` | `6152` | `29511` |
| **27%** | [Koa](#koa) | `19679` | `7895` | `58848` |
| **11%** | [Carbon](#carbon) | `8161` | `1343` | `10282` |
| **9%** | [Express](#express) | `6570` | `1124` | `8462` |


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
    Reqs/sec      8735.83    4627.10   55693.61
    Latency        5.71ms     4.26ms   367.16ms
    HTTP codes:
      1xx - 0, 2xx - 92866, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7134
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7134
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
    Reqs/sec      6311.89    1006.92    8449.63
    Latency        7.92ms     3.63ms   351.54ms
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
    Reqs/sec     22997.04    6501.80   35334.85
    Latency        2.17ms     2.08ms   185.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.22MB/s
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
    Reqs/sec     21180.30    6020.98   29530.61
    Latency        2.36ms     2.06ms   187.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     61255.36    3147.96   64239.17
    Latency      814.26us    71.08us     2.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.70MB/s
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
    Reqs/sec     18498.62    6833.04   58924.24
    Latency        2.70ms     2.30ms   202.88ms
    HTTP codes:
      1xx - 0, 2xx - 94204, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5796
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5796
    Throughput:     3.94MB/s
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
    Reqs/sec     25143.19    7098.84   58929.45
    Latency        1.98ms     1.88ms   163.28ms
    HTTP codes:
      1xx - 0, 2xx - 97273, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2727
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2727
    Throughput:     5.60MB/s
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
    Reqs/sec     73643.33    5027.22   84576.02
    Latency      676.03us   163.51us     8.34ms
    HTTP codes:
      1xx - 0, 2xx - 97816, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2184
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2184
    Throughput:    11.40MB/s
  ```


