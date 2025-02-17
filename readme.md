## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75031` | `5057` | `84201` |
| **84%** | [Hyper Express](#hyper-express) | `62826` | `4277` | `71115` |
| **36%** | [Node (Default)](#node-default) | `26759` | `7628` | `53598` |
| **34%** | [Fastify](#fastify) | `25586` | `7863` | `36086` |
| **27%** | [Hono](#hono) | `20530` | `5611` | `30497` |
| **25%** | [Koa](#koa) | `19101` | `6740` | `57707` |
| **11%** | [Carbon](#carbon) | `8339` | `1440` | `10328` |
| **9%** | [Express](#express) | `6489` | `1057` | `8496` |


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
    Reqs/sec      8931.39    5133.74   55823.70
    Latency        5.59ms     4.30ms   372.31ms
    HTTP codes:
      1xx - 0, 2xx - 92155, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7845
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7845
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
    Reqs/sec      6892.33    4009.80   47822.29
    Latency        7.24ms     3.97ms   361.20ms
    HTTP codes:
      1xx - 0, 2xx - 93081, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6919
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6919
    Throughput:     1.84MB/s
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
    Reqs/sec     24447.08    7332.77   37181.01
    Latency        2.04ms     1.84ms   168.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.55MB/s
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
    Reqs/sec     20665.25    5383.85   29975.74
    Latency        2.42ms     2.01ms   180.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     63908.96    4010.36   70980.59
    Latency      780.32us    74.65us     3.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.08MB/s
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
    Reqs/sec     19117.46    7675.22   62389.19
    Latency        2.61ms     2.36ms   204.70ms
    HTTP codes:
      1xx - 0, 2xx - 90737, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9263
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9263
    Throughput:     3.92MB/s
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
    Reqs/sec     27098.61    7631.31   52168.70
    Latency        1.84ms     1.72ms   148.42ms
    HTTP codes:
      1xx - 0, 2xx - 97721, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2279
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2279
    Throughput:     6.07MB/s
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
    Reqs/sec     74405.62    5390.21   81885.96
    Latency      669.42us   177.73us     9.55ms
    HTTP codes:
      1xx - 0, 2xx - 96796, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3204
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3204
    Throughput:    11.40MB/s
  ```


