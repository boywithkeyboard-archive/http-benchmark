## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73638` | `4548` | `81035` |
| **85%** | [Hyper Express](#hyper-express) | `62759` | `4500` | `88207` |
| **34%** | [Node (Default)](#node-default) | `25152` | `7275` | `60453` |
| **34%** | [Fastify](#fastify) | `24781` | `7285` | `36133` |
| **28%** | [Hono](#hono) | `20887` | `5639` | `29201` |
| **25%** | [Koa](#koa) | `18610` | `8131` | `64816` |
| **11%** | [Carbon](#carbon) | `8102` | `1335` | `10285` |
| **9%** | [Express](#express) | `6454` | `1063` | `8320` |


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
    Reqs/sec      8583.29    4402.79   54700.69
    Latency        5.82ms     4.26ms   364.58ms
    HTTP codes:
      1xx - 0, 2xx - 93490, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6510
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6510
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
    Reqs/sec      6417.58    1033.53    8297.50
    Latency        7.79ms     3.66ms   350.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     22551.89    6290.58   35658.69
    Latency        2.22ms     2.05ms   182.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.11MB/s
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
    Reqs/sec     20312.03    5704.79   29515.80
    Latency        2.46ms     2.06ms   185.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     62205.61    3576.71   64548.38
    Latency      801.14us    76.99us     3.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     19011.34    7299.94   53616.06
    Latency        2.62ms     2.38ms   209.98ms
    HTTP codes:
      1xx - 0, 2xx - 91452, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8548
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8548
    Throughput:     3.93MB/s
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
    Reqs/sec     27514.16    8531.06   53735.95
    Latency        1.81ms     1.96ms   166.93ms
    HTTP codes:
      1xx - 0, 2xx - 97611, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2389
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2389
    Throughput:     6.15MB/s
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
    Reqs/sec     74204.35    4860.09   87079.61
    Latency      672.30us   169.66us    10.22ms
    HTTP codes:
      1xx - 0, 2xx - 97871, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2129
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2129
    Throughput:    11.48MB/s
  ```


