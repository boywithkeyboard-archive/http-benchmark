## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71768` | `4251` | `90750` |
| **82%** | [Hyper Express](#hyper-express) | `58510` | `5248` | `67334` |
| **28%** | [Node (Default)](#node-default) | `20212` | `5337` | `60827` |
| **25%** | [Fastify](#fastify) | `18217` | `3566` | `29561` |
| **23%** | [Hono](#hono) | `16634` | `3587` | `29915` |
| **21%** | [Koa](#koa) | `15318` | `6266` | `63908` |
| **10%** | [Carbon](#carbon) | `7333` | `1206` | `10566` |
| **9%** | [Express](#express) | `6153` | `1110` | `8266` |


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
    Reqs/sec      8094.59    5812.67   69339.99
    Latency        6.17ms     4.61ms   391.73ms
    HTTP codes:
      1xx - 0, 2xx - 90851, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9149
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9149
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
    Reqs/sec      6084.68    1069.97    8264.33
    Latency        8.21ms     3.90ms   374.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     18815.92    3655.33   31737.70
    Latency        2.65ms     2.09ms   189.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.27MB/s
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
    Reqs/sec     18505.55    4241.55   30239.60
    Latency        2.70ms     2.20ms   194.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.18MB/s
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
    Reqs/sec     59197.12    2900.72   64621.25
    Latency      842.34us    85.71us     4.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.41MB/s
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
    Reqs/sec     15496.84    7100.51   67703.24
    Latency        3.22ms     2.50ms   216.78ms
    HTTP codes:
      1xx - 0, 2xx - 92404, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7596
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7596
    Throughput:     3.24MB/s
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
    Reqs/sec     20024.02    4967.43   54333.00
    Latency        2.49ms     2.02ms   175.75ms
    HTTP codes:
      1xx - 0, 2xx - 97702, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2298
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2298
    Throughput:     4.48MB/s
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
    Reqs/sec     70967.12    4371.83   81995.65
    Latency      701.06us   198.69us    11.79ms
    HTTP codes:
      1xx - 0, 2xx - 96467, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3533
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3533
    Throughput:    10.84MB/s
  ```


