## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68352` | `3974` | `77771` |
| **85%** | [Hyper Express](#hyper-express) | `57829` | `4384` | `70157` |
| **30%** | [Hono](#hono) | `20693` | `6571` | `30328` |
| **30%** | [Node (Default)](#node-default) | `20405` | `4960` | `55306` |
| **30%** | [Fastify](#fastify) | `20225` | `5568` | `36061` |
| **24%** | [Koa](#koa) | `16446` | `7060` | `56079` |
| **11%** | [Carbon](#carbon) | `7248` | `1233` | `10275` |
| **9%** | [Express](#express) | `5993` | `1060` | `8110` |


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
    Reqs/sec      7787.70    5408.65   73578.39
    Latency        6.41ms     4.51ms   387.96ms
    HTTP codes:
      1xx - 0, 2xx - 91493, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8507
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8507
    Throughput:     1.62MB/s
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
    Reqs/sec      5978.22    1052.33    8073.25
    Latency        8.36ms     3.85ms   370.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.71MB/s
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
    Reqs/sec     20519.64    5189.81   37080.88
    Latency        2.44ms     2.14ms   191.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     21278.42    6466.27   30358.06
    Latency        2.35ms     2.42ms   209.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     57704.17    3834.01   66313.74
    Latency        0.86ms   102.67us     3.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.20MB/s
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
    Reqs/sec     19985.36    9989.04   75153.49
    Latency        2.50ms     2.63ms   229.17ms
    HTTP codes:
      1xx - 0, 2xx - 89803, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10197
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10197
    Throughput:     4.06MB/s
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
    Reqs/sec     20096.92    5433.78   71605.26
    Latency        2.48ms     2.08ms   181.40ms
    HTTP codes:
      1xx - 0, 2xx - 96427, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3573
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3573
    Throughput:     4.45MB/s
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
    Reqs/sec     68608.39    4182.57   81533.65
    Latency      724.86us   175.90us     9.39ms
    HTTP codes:
      1xx - 0, 2xx - 94648, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5352
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5352
    Throughput:    10.28MB/s
  ```


