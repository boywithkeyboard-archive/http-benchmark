## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73862` | `5252` | `79115` |
| **84%** | [Hyper Express](#hyper-express) | `62017` | `2801` | `64952` |
| **34%** | [Node (Default)](#node-default) | `25078` | `7098` | `59224` |
| **32%** | [Fastify](#fastify) | `24000` | `6875` | `35293` |
| **28%** | [Hono](#hono) | `20690` | `5683` | `30813` |
| **24%** | [Koa](#koa) | `17698` | `5640` | `41268` |
| **11%** | [Carbon](#carbon) | `8192` | `1430` | `10504` |
| **9%** | [Express](#express) | `6513` | `1008` | `8530` |


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
    Reqs/sec      8860.99    4980.69   69417.42
    Latency        5.63ms     4.11ms   355.72ms
    HTTP codes:
      1xx - 0, 2xx - 92639, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7361
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7361
    Throughput:     1.86MB/s
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
    Reqs/sec      6528.03    1048.53    8446.31
    Latency        7.66ms     3.50ms   337.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24285.17    7510.61   36027.72
    Latency        2.06ms     2.01ms   178.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     19858.36    5164.79   29613.66
    Latency        2.51ms     1.89ms   172.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.49MB/s
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
    Reqs/sec     62880.28    3825.40   70898.72
    Latency      792.83us    69.46us     3.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     17877.80    6008.29   48008.96
    Latency        2.79ms     2.28ms   201.53ms
    HTTP codes:
      1xx - 0, 2xx - 94804, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5196
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5196
    Throughput:     3.84MB/s
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
    Reqs/sec     26365.14    7720.91   49071.42
    Latency        1.89ms     1.98ms   170.78ms
    HTTP codes:
      1xx - 0, 2xx - 97899, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2101
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2073
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 28
    Throughput:     5.92MB/s
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
    Reqs/sec     73303.88    5350.50   80845.61
    Latency      680.35us   201.36us     8.55ms
    HTTP codes:
      1xx - 0, 2xx - 96878, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3122
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3122
    Throughput:    11.23MB/s
  ```


