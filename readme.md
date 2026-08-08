## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73643` | `3520` | `81772` |
| **82%** | [Hyper Express](#hyper-express) | `60740` | `3634` | `68973` |
| **31%** | [Hono](#hono) | `22891` | `6530` | `30750` |
| **31%** | [Fastify](#fastify) | `22853` | `6235` | `36702` |
| **30%** | [Node (Default)](#node-default) | `22129` | `6271` | `62855` |
| **26%** | [Koa](#koa) | `18937` | `7920` | `65021` |
| **11%** | [Carbon](#carbon) | `7827` | `1380` | `10651` |
| **9%** | [Express](#express) | `6396` | `1085` | `8494` |


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
    Reqs/sec      8297.81    5226.42   72869.19
    Latency        6.01ms     4.47ms   381.11ms
    HTTP codes:
      1xx - 0, 2xx - 92708, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7292
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7292
    Throughput:     1.75MB/s
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
    Reqs/sec      6544.81    1131.08    8459.31
    Latency        7.64ms     3.79ms   356.94ms
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
    Reqs/sec     22335.27    6159.33   37912.99
    Latency        2.24ms     2.05ms   184.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.06MB/s
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
    Reqs/sec     22479.28    6107.51   30937.46
    Latency        2.22ms     2.27ms   194.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.08MB/s
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
    Reqs/sec     61013.92    3735.80   65482.10
    Latency      817.29us   106.07us     4.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.67MB/s
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
    Reqs/sec     19476.28    8146.48   71756.80
    Latency        2.56ms     2.38ms   207.57ms
    HTTP codes:
      1xx - 0, 2xx - 93158, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6842
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6842
    Throughput:     4.10MB/s
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
    Reqs/sec     24268.44    8460.16   80242.70
    Latency        2.05ms     1.92ms   164.28ms
    HTTP codes:
      1xx - 0, 2xx - 94421, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5579
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5579
    Throughput:     5.25MB/s
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
    Reqs/sec     74247.62    4353.29   86225.49
    Latency      669.91us   214.66us    13.31ms
    HTTP codes:
      1xx - 0, 2xx - 94533, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5467
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5467
    Throughput:    11.11MB/s
  ```


