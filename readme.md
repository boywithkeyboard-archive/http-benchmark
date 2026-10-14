## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `124389` | `3083` | `129867` |
| **93%** | [Hyper Express](#hyper-express) | `115897` | `5531` | `120723` |
| **57%** | [Fastify](#fastify) | `70880` | `17933` | `88515` |
| **56%** | [Node (Default)](#node-default) | `69968` | `16341` | `133338` |
| **55%** | [Koa](#koa) | `67809` | `19908` | `132643` |
| **54%** | [Hono](#hono) | `66972` | `17828` | `86726` |
| **20%** | [Carbon](#carbon) | `25389` | `8432` | `38867` |
| **15%** | [Express](#express) | `18289` | `4428` | `26267` |


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
    Reqs/sec     27992.48   14729.04  129976.43
    Latency        1.78ms     2.93ms   246.08ms
    HTTP codes:
      1xx - 0, 2xx - 91189, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8811
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8811
    Throughput:     5.80MB/s
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
    Reqs/sec     19913.04   12014.27  123279.45
    Latency        2.51ms     2.28ms   201.47ms
    HTTP codes:
      1xx - 0, 2xx - 90895, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9105
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9105
    Throughput:     5.18MB/s
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
    Reqs/sec     75829.18   21828.08  126661.12
    Latency      657.84us     1.02ms    82.17ms
    HTTP codes:
      1xx - 0, 2xx - 83682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 16318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 16318
    Throughput:    14.40MB/s
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
    Reqs/sec     69017.69   19433.05  113520.92
    Latency      719.45us     1.08ms    88.36ms
    HTTP codes:
      1xx - 0, 2xx - 93078, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6922
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6922
    Throughput:    14.56MB/s
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
    Reqs/sec    117931.53    5087.00  120942.58
    Latency      421.77us   160.46us     7.73ms
    HTTP codes:
      1xx - 0, 2xx - 94592, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5408
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5408
    Throughput:    15.85MB/s
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
    Reqs/sec     62859.63   18605.25  131625.30
    Latency      792.79us     1.10ms    88.20ms
    HTTP codes:
      1xx - 0, 2xx - 91938, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8062
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8062
    Throughput:    13.07MB/s
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
    Reqs/sec     74260.72   17223.15  119009.52
    Latency      672.55us   784.36us    63.79ms
    HTTP codes:
      1xx - 0, 2xx - 95862, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4138
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4138
    Throughput:    16.27MB/s
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
    Reqs/sec    125458.03    3868.28  130063.39
    Latency      396.52us   126.55us     6.52ms
    HTTP codes:
      1xx - 0, 2xx - 95394, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4606
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4606
    Throughput:    18.94MB/s
  ```


