## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74886` | `5209` | `80894` |
| **84%** | [Hyper Express](#hyper-express) | `63093` | `4296` | `67330` |
| **34%** | [Node (Default)](#node-default) | `25771` | `7102` | `44954` |
| **34%** | [Fastify](#fastify) | `25161` | `7530` | `36323` |
| **29%** | [Hono](#hono) | `21602` | `5601` | `29856` |
| **24%** | [Koa](#koa) | `18331` | `6173` | `52352` |
| **11%** | [Carbon](#carbon) | `8551` | `1486` | `10704` |
| **9%** | [Express](#express) | `6599` | `1057` | `8615` |


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
    Reqs/sec      8744.54    4728.56   54901.14
    Latency        5.71ms     4.17ms   365.81ms
    HTTP codes:
      1xx - 0, 2xx - 93023, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6977
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6977
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
    Reqs/sec      6777.19    1126.84    8663.46
    Latency        7.37ms     3.44ms   329.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.94MB/s
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
    Reqs/sec     24266.31    7400.03   35507.95
    Latency        2.06ms     1.99ms   175.73ms
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
    Reqs/sec     21070.60    5626.30   29203.35
    Latency        2.37ms     1.93ms   176.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     62973.37    3872.85   66907.61
    Latency      791.73us    68.64us     3.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.94MB/s
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
    Reqs/sec     17696.44    6242.62   52710.26
    Latency        2.82ms     2.35ms   204.71ms
    HTTP codes:
      1xx - 0, 2xx - 94413, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5587
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5587
    Throughput:     3.78MB/s
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
    Reqs/sec     25347.36    7143.63   48687.47
    Latency        1.97ms     1.94ms   164.13ms
    HTTP codes:
      1xx - 0, 2xx - 98010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1990
    Throughput:     5.69MB/s
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
    Reqs/sec     75613.55    6477.06   87276.46
    Latency      658.25us   233.77us    12.74ms
    HTTP codes:
      1xx - 0, 2xx - 98272, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1728
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1728
    Throughput:    11.76MB/s
  ```


