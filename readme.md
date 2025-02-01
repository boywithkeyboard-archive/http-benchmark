## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73197` | `6829` | `83770` |
| **81%** | [Hyper Express](#hyper-express) | `59409` | `4134` | `71942` |
| **35%** | [Node (Default)](#node-default) | `25284` | `7805` | `51773` |
| **32%** | [Fastify](#fastify) | `23481` | `6950` | `36194` |
| **28%** | [Hono](#hono) | `20497` | `6332` | `29925` |
| **25%** | [Koa](#koa) | `18121` | `6726` | `53522` |
| **11%** | [Carbon](#carbon) | `7952` | `1411` | `10249` |
| **8%** | [Express](#express) | `6215` | `1029` | `8447` |


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
    Reqs/sec      8574.30    4300.41   54429.28
    Latency        5.82ms     4.40ms   379.99ms
    HTTP codes:
      1xx - 0, 2xx - 93676, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6324
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6324
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
    Reqs/sec      6206.06    1072.31    8462.09
    Latency        8.05ms     3.95ms   372.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     24112.94    8040.13   35278.57
    Latency        2.07ms     2.18ms   194.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     20910.78    6235.75   30995.01
    Latency        2.39ms     2.03ms   183.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     59483.79    3444.59   63661.11
    Latency      839.10us    73.62us     2.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.44MB/s
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
    Reqs/sec     18943.17    7085.06   55312.15
    Latency        2.63ms     2.40ms   209.72ms
    HTTP codes:
      1xx - 0, 2xx - 93396, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6604
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6604
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
    Reqs/sec     24929.48    6693.64   43072.84
    Latency        2.00ms     1.86ms   159.43ms
    HTTP codes:
      1xx - 0, 2xx - 97952, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2048
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2048
    Throughput:     5.59MB/s
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
    Reqs/sec     72724.08    4502.03   80225.76
    Latency      685.23us   183.01us     9.40ms
    HTTP codes:
      1xx - 0, 2xx - 97432, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2568
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2568
    Throughput:    11.21MB/s
  ```


