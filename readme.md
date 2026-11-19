## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `100254` | `4626` | `109288` |
| **88%** | [Hyper Express](#hyper-express) | `88045` | `5367` | `112955` |
| **50%** | [Fastify](#fastify) | `50017` | `16401` | `65645` |
| **48%** | [Node (Default)](#node-default) | `48547` | `14732` | `100170` |
| **40%** | [Hono](#hono) | `40440` | `14417` | `59547` |
| **38%** | [Koa](#koa) | `37617` | `15522` | `103126` |
| **11%** | [Carbon](#carbon) | `11197` | `2445` | `17094` |
| **9%** | [Express](#express) | `9352` | `1835` | `13485` |


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
    Reqs/sec     12647.49    9219.90  108440.29
    Latency        3.93ms     3.35ms   284.35ms
    HTTP codes:
      1xx - 0, 2xx - 90102, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9898
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9898
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
    Reqs/sec     10661.66    9861.90   95086.88
    Latency        4.68ms     3.05ms   273.93ms
    HTTP codes:
      1xx - 0, 2xx - 87394, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12606
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12606
    Throughput:     2.67MB/s
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
    Reqs/sec     48745.93   16804.98   65986.24
    Latency        1.02ms     1.57ms   137.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    11.06MB/s
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
    Reqs/sec     42915.43   14854.60   59050.49
    Latency        1.17ms     1.60ms   134.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.68MB/s
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
    Reqs/sec     88268.64    3878.59  102859.27
    Latency      564.76us    55.29us     2.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.54MB/s
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
    Reqs/sec     38597.74   15428.57   94772.97
    Latency        1.29ms     1.90ms   171.02ms
    HTTP codes:
      1xx - 0, 2xx - 92870, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7130
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7130
    Throughput:     8.10MB/s
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
    Reqs/sec     47434.72   14579.51   87268.79
    Latency        1.05ms     1.36ms   113.85ms
    HTTP codes:
      1xx - 0, 2xx - 96494, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3506
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3506
    Throughput:    10.49MB/s
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
    Reqs/sec    101859.36    4552.08  107812.61
    Latency      487.21us   180.92us    11.63ms
    HTTP codes:
      1xx - 0, 2xx - 96300, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3700
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3700
    Throughput:    15.53MB/s
  ```


