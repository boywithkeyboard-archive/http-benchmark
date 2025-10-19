## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74302` | `4041` | `83979` |
| **83%** | [Hyper Express](#hyper-express) | `61639` | `3943` | `76948` |
| **31%** | [Node (Default)](#node-default) | `22797` | `6001` | `71505` |
| **30%** | [Fastify](#fastify) | `22382` | `6694` | `36243` |
| **27%** | [Hono](#hono) | `20214` | `6231` | `30526` |
| **23%** | [Koa](#koa) | `17379` | `7965` | `68731` |
| **11%** | [Carbon](#carbon) | `7902` | `1432` | `10112` |
| **8%** | [Express](#express) | `6107` | `1071` | `8072` |


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
    Reqs/sec      8730.04    6851.90   71254.00
    Latency        5.72ms     4.48ms   388.00ms
    HTTP codes:
      1xx - 0, 2xx - 89229, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10771
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10771
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
    Reqs/sec      6055.90    1053.27    7982.35
    Latency        8.25ms     3.73ms   364.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     22207.57    6727.35   35611.32
    Latency        2.25ms     2.16ms   194.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.04MB/s
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
    Reqs/sec     20789.76    6386.81   29503.61
    Latency        2.40ms     2.17ms   193.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     61262.69    3861.43   66421.80
    Latency      813.79us    90.72us     3.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.70MB/s
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
    Reqs/sec     17668.10    7815.22   63215.84
    Latency        2.82ms     2.51ms   224.56ms
    HTTP codes:
      1xx - 0, 2xx - 92862, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7138
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7138
    Throughput:     3.71MB/s
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
    Reqs/sec     22919.00    6735.62   60026.94
    Latency        2.18ms     2.05ms   178.89ms
    HTTP codes:
      1xx - 0, 2xx - 97421, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2579
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2579
    Throughput:     5.11MB/s
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
    Reqs/sec     75205.18    4842.72   95198.65
    Latency      662.65us   135.03us     8.26ms
    HTTP codes:
      1xx - 0, 2xx - 97469, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2531
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2531
    Throughput:    11.60MB/s
  ```


