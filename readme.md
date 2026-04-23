## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `107350` | `4178` | `115056` |
| **90%** | [Hyper Express](#hyper-express) | `96094` | `4976` | `109198` |
| **45%** | [Node (Default)](#node-default) | `48679` | `16814` | `117401` |
| **43%** | [Fastify](#fastify) | `46165` | `15072` | `67595` |
| **38%** | [Hono](#hono) | `40311` | `12234` | `62807` |
| **34%** | [Koa](#koa) | `36170` | `15234` | `100319` |
| **11%** | [Carbon](#carbon) | `11943` | `2724` | `17047` |
| **9%** | [Express](#express) | `9150` | `1785` | `12907` |


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
    Reqs/sec     13729.50    9811.73   99198.21
    Latency        3.63ms     3.24ms   280.41ms
    HTTP codes:
      1xx - 0, 2xx - 89829, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10171
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10171
    Throughput:     2.81MB/s
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
    Reqs/sec     11223.32    9939.84  100589.31
    Latency        4.45ms     2.95ms   267.90ms
    HTTP codes:
      1xx - 0, 2xx - 87916, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12084
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12084
    Throughput:     2.82MB/s
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
    Reqs/sec     47948.66   19851.01  101713.52
    Latency        1.04ms     1.60ms   132.81ms
    HTTP codes:
      1xx - 0, 2xx - 80090, 3xx - 0, 4xx - 0, 5xx - 0
      others - 19910
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 19910
    Throughput:     8.72MB/s
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
    Reqs/sec     43229.55   14163.84  102657.23
    Latency        1.15ms     1.51ms   128.75ms
    HTTP codes:
      1xx - 0, 2xx - 92101, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7899
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7899
    Throughput:     9.01MB/s
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
    Reqs/sec     93503.74    5046.84   97673.27
    Latency      532.47us   183.71us     9.03ms
    HTTP codes:
      1xx - 0, 2xx - 92261, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7739
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7739
    Throughput:    12.25MB/s
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
    Reqs/sec     41042.56   17558.55  116450.00
    Latency        1.22ms     1.77ms   149.05ms
    HTTP codes:
      1xx - 0, 2xx - 89780, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10220
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10220
    Throughput:     8.33MB/s
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
    Reqs/sec     50484.89   15361.75  103811.64
    Latency        0.99ms     1.31ms   106.89ms
    HTTP codes:
      1xx - 0, 2xx - 94270, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5730
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5730
    Throughput:    10.87MB/s
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
    Reqs/sec    107265.33    3630.52  112810.07
    Latency      464.20us   124.82us     6.23ms
    HTTP codes:
      1xx - 0, 2xx - 95983, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4017
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4017
    Throughput:    16.30MB/s
  ```


