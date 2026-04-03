## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70896` | `3371` | `81364` |
| **83%** | [Hyper Express](#hyper-express) | `58975` | `3557` | `66176` |
| **30%** | [Fastify](#fastify) | `21401` | `6331` | `37513` |
| **29%** | [Hono](#hono) | `20680` | `6376` | `29960` |
| **28%** | [Node (Default)](#node-default) | `19701` | `5063` | `56387` |
| **26%** | [Koa](#koa) | `18111` | `8688` | `67683` |
| **11%** | [Carbon](#carbon) | `7454` | `1288` | `10263` |
| **9%** | [Express](#express) | `6144` | `1103` | `8323` |


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
    Reqs/sec      8165.36    6350.27   67677.89
    Latency        6.11ms     4.41ms   381.93ms
    HTTP codes:
      1xx - 0, 2xx - 90012, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9988
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9988
    Throughput:     1.67MB/s
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
    Reqs/sec      6131.93    1039.10    8192.68
    Latency        8.15ms     3.76ms   362.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     21233.30    6691.62   36816.63
    Latency        2.35ms     2.05ms   185.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     22845.07    7262.73   31221.73
    Latency        2.19ms     2.16ms   194.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.16MB/s
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
    Reqs/sec     60264.55    3797.62   71242.36
    Latency      828.42us    83.79us     3.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.55MB/s
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
    Reqs/sec     18259.12    8273.70   69187.95
    Latency        2.73ms     2.44ms   213.33ms
    HTTP codes:
      1xx - 0, 2xx - 93067, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6933
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6933
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
    Reqs/sec     20901.68    5396.33   67881.25
    Latency        2.39ms     1.90ms   162.93ms
    HTTP codes:
      1xx - 0, 2xx - 96973, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3027
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3027
    Throughput:     4.64MB/s
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
    Reqs/sec     70261.44    4385.42   83354.81
    Latency      709.24us   184.23us     9.46ms
    HTTP codes:
      1xx - 0, 2xx - 96819, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3181
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3181
    Throughput:    10.76MB/s
  ```


