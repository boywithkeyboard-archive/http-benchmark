## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74799` | `5868` | `84940` |
| **84%** | [Hyper Express](#hyper-express) | `62807` | `3780` | `68411` |
| **35%** | [Node (Default)](#node-default) | `25870` | `7076` | `50831` |
| **31%** | [Fastify](#fastify) | `23387` | `6468` | `35962` |
| **27%** | [Hono](#hono) | `20305` | `5818` | `30247` |
| **25%** | [Koa](#koa) | `18572` | `6323` | `47223` |
| **11%** | [Carbon](#carbon) | `8426` | `1406` | `10685` |
| **9%** | [Express](#express) | `6457` | `1012` | `8539` |


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
    Reqs/sec      8656.93    3720.45   49734.32
    Latency        5.75ms     4.15ms   361.31ms
    HTTP codes:
      1xx - 0, 2xx - 94668, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5332
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5332
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
    Reqs/sec      6702.72    1091.78    8651.11
    Latency        7.46ms     3.55ms   338.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.92MB/s
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
    Reqs/sec     24171.18    7204.22   36098.52
    Latency        2.07ms     1.97ms   178.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     21300.65    5189.75   29905.14
    Latency        2.34ms     1.90ms   173.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.81MB/s
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
    Reqs/sec     64009.10    3412.77   67705.60
    Latency      778.89us    60.20us     3.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.09MB/s
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
    Reqs/sec     17806.21    5789.14   44987.19
    Latency        2.80ms     2.32ms   203.80ms
    HTTP codes:
      1xx - 0, 2xx - 95029, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4971
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4971
    Throughput:     3.83MB/s
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
    Reqs/sec     26922.64    7721.77   42426.98
    Latency        1.85ms     1.90ms   164.05ms
    HTTP codes:
      1xx - 0, 2xx - 98285, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1715
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1715
    Throughput:     6.06MB/s
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
    Reqs/sec     74095.90    5455.57   83658.23
    Latency      672.90us   197.07us    10.65ms
    HTTP codes:
      1xx - 0, 2xx - 97009, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2991
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2991
    Throughput:    11.37MB/s
  ```


