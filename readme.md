## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72489` | `2247` | `82036` |
| **81%** | [Hyper Express](#hyper-express) | `58960` | `3361` | `61881` |
| **34%** | [Node (Default)](#node-default) | `24387` | `7591` | `58391` |
| **32%** | [Fastify](#fastify) | `23397` | `7841` | `36589` |
| **28%** | [Hono](#hono) | `19958` | `5462` | `29352` |
| **25%** | [Koa](#koa) | `17961` | `8121` | `70918` |
| **11%** | [Carbon](#carbon) | `7845` | `1493` | `10271` |
| **8%** | [Express](#express) | `6081` | `1019` | `8227` |


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
    Reqs/sec      8699.12    6901.90   79946.20
    Latency        5.73ms     4.39ms   379.04ms
    HTTP codes:
      1xx - 0, 2xx - 89516, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10484
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10484
    Throughput:     1.77MB/s
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
    Reqs/sec      6290.79    1096.94    8239.50
    Latency        7.95ms     3.74ms   357.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     22396.01    6845.48   35259.90
    Latency        2.23ms     2.05ms   181.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.08MB/s
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
    Reqs/sec     20638.13    6242.51   29597.71
    Latency        2.42ms     2.04ms   184.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     58970.00    3109.04   60897.17
    Latency      845.46us    74.51us     3.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.38MB/s
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
    Reqs/sec     17813.49    7780.54   66070.38
    Latency        2.80ms     2.43ms   215.15ms
    HTTP codes:
      1xx - 0, 2xx - 93334, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6666
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6666
    Throughput:     3.76MB/s
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
    Reqs/sec     26064.62    8788.81   64040.88
    Latency        1.91ms     1.89ms   161.44ms
    HTTP codes:
      1xx - 0, 2xx - 97299, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2701
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2701
    Throughput:     5.81MB/s
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
    Reqs/sec     74000.46    2214.26   80054.75
    Latency      672.78us   135.20us     7.27ms
    HTTP codes:
      1xx - 0, 2xx - 96477, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3523
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3523
    Throughput:    11.30MB/s
  ```


