## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74144` | `5145` | `82390` |
| **86%** | [Hyper Express](#hyper-express) | `63753` | `4182` | `70270` |
| **34%** | [Node (Default)](#node-default) | `25065` | `6607` | `42871` |
| **33%** | [Fastify](#fastify) | `24618` | `7213` | `36012` |
| **27%** | [Hono](#hono) | `20289` | `5236` | `29636` |
| **25%** | [Koa](#koa) | `18349` | `7069` | `56612` |
| **11%** | [Carbon](#carbon) | `8243` | `1369` | `10494` |
| **9%** | [Express](#express) | `6595` | `1134` | `19114` |


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
    Reqs/sec      8926.70    3999.06   51739.43
    Latency        5.59ms     4.19ms   361.73ms
    HTTP codes:
      1xx - 0, 2xx - 94655, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5345
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5345
    Throughput:     1.92MB/s
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
    Reqs/sec      7048.67    3831.41   46642.58
    Latency        7.08ms     3.85ms   351.31ms
    HTTP codes:
      1xx - 0, 2xx - 93242, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6758
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6755
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 3
    Throughput:     1.88MB/s
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
    Reqs/sec     24486.84    7562.28   35617.40
    Latency        2.04ms     1.97ms   179.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     19983.11    5216.03   29245.85
    Latency        2.50ms     1.89ms   172.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.51MB/s
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
    Reqs/sec     62989.46    4262.55   66751.34
    Latency      791.42us    76.01us     4.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     18056.94    5884.54   41661.17
    Latency        2.76ms     2.32ms   202.45ms
    HTTP codes:
      1xx - 0, 2xx - 95514, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4486
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4486
    Throughput:     3.90MB/s
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
    Reqs/sec     27792.92    8440.12   57871.18
    Latency        1.79ms     1.80ms   153.48ms
    HTTP codes:
      1xx - 0, 2xx - 97240, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2760
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2760
    Throughput:     6.19MB/s
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
    Reqs/sec     73473.03    5633.23   90291.65
    Latency      678.83us   191.31us    17.21ms
    HTTP codes:
      1xx - 0, 2xx - 97863, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2137
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2137
    Throughput:    11.37MB/s
  ```


