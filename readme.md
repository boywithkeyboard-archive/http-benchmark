## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72928` | `4056` | `75672` |
| **84%** | [Hyper Express](#hyper-express) | `61062` | `3948` | `73235` |
| **34%** | [Node (Default)](#node-default) | `25079` | `6734` | `50233` |
| **32%** | [Fastify](#fastify) | `23551` | `7331` | `37272` |
| **29%** | [Hono](#hono) | `21010` | `5989` | `30712` |
| **26%** | [Koa](#koa) | `18619` | `6271` | `45232` |
| **11%** | [Carbon](#carbon) | `8045` | `1333` | `10364` |
| **9%** | [Express](#express) | `6375` | `1045` | `8387` |


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
    Reqs/sec      8447.16    3507.60   52321.71
    Latency        5.90ms     4.28ms   368.89ms
    HTTP codes:
      1xx - 0, 2xx - 94864, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5136
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5136
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
    Reqs/sec      6461.02    1074.78    8445.12
    Latency        7.73ms     3.69ms   353.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     23495.92    7203.89   36296.20
    Latency        2.13ms     2.07ms   182.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.33MB/s
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
    Reqs/sec     21126.81    6425.20   31330.68
    Latency        2.36ms     2.05ms   186.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     61858.03    3581.35   64279.08
    Latency      806.09us    71.22us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.79MB/s
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
    Reqs/sec     18781.89    6834.86   53720.47
    Latency        2.66ms     2.36ms   206.95ms
    HTTP codes:
      1xx - 0, 2xx - 94164, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5836
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5836
    Throughput:     4.00MB/s
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
    Reqs/sec     25342.38    6844.50   48312.55
    Latency        1.97ms     1.89ms   162.78ms
    HTTP codes:
      1xx - 0, 2xx - 98010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1990
    Throughput:     5.68MB/s
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
    Reqs/sec     73249.96    4939.53   82611.94
    Latency      680.11us   173.68us     6.40ms
    HTTP codes:
      1xx - 0, 2xx - 97260, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2740
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2740
    Throughput:    11.28MB/s
  ```


