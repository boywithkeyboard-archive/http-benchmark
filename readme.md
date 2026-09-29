## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68538` | `3640` | `79914` |
| **84%** | [Hyper Express](#hyper-express) | `57400` | `3447` | `65319` |
| **31%** | [Hono](#hono) | `21082` | `6806` | `30342` |
| **29%** | [Node (Default)](#node-default) | `20090` | `5772` | `67267` |
| **28%** | [Fastify](#fastify) | `19434` | `4080` | `31749` |
| **27%** | [Koa](#koa) | `18668` | `9589` | `77928` |
| **11%** | [Carbon](#carbon) | `7335` | `1206` | `10388` |
| **9%** | [Express](#express) | `6179` | `1117` | `8345` |


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
    Reqs/sec      7949.32    5729.75   62513.30
    Latency        6.27ms     4.61ms   394.61ms
    HTTP codes:
      1xx - 0, 2xx - 90390, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9610
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9610
    Throughput:     1.63MB/s
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
    Reqs/sec      6175.62    1092.60    8238.10
    Latency        8.09ms     3.78ms   366.79ms
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
    Reqs/sec     19589.24    4149.87   32057.67
    Latency        2.55ms     2.14ms   190.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.44MB/s
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
    Reqs/sec     20783.74    6201.59   30548.89
    Latency        2.40ms     2.29ms   202.54ms
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
    Reqs/sec     57055.27    3395.20   61747.86
    Latency        0.87ms    99.74us     3.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.10MB/s
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
    Reqs/sec     18580.89    7859.68   61014.20
    Latency        2.68ms     2.49ms   216.00ms
    HTTP codes:
      1xx - 0, 2xx - 93413, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6587
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6587
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
    Reqs/sec     19852.58    4776.79   62129.30
    Latency        2.51ms     2.08ms   180.35ms
    HTTP codes:
      1xx - 0, 2xx - 97606, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2394
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2394
    Throughput:     4.44MB/s
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
    Reqs/sec     68904.42    3863.03   78890.78
    Latency      722.77us   176.00us     7.38ms
    HTTP codes:
      1xx - 0, 2xx - 96745, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3255
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3255
    Throughput:    10.55MB/s
  ```


