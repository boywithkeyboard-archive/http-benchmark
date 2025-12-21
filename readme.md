## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74943` | `3055` | `80962` |
| **85%** | [Hyper Express](#hyper-express) | `63541` | `3808` | `71808` |
| **33%** | [Node (Default)](#node-default) | `25055` | `7638` | `62186` |
| **32%** | [Fastify](#fastify) | `24174` | `7531` | `35854` |
| **29%** | [Hono](#hono) | `21561` | `6352` | `30590` |
| **25%** | [Koa](#koa) | `18822` | `8442` | `75007` |
| **11%** | [Carbon](#carbon) | `8134` | `1428` | `10399` |
| **8%** | [Express](#express) | `6333` | `1008` | `8317` |


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
    Reqs/sec      9058.70    6151.26   78239.57
    Latency        5.51ms     4.28ms   367.79ms
    HTTP codes:
      1xx - 0, 2xx - 91370, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8630
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8630
    Throughput:     1.88MB/s
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
    Reqs/sec      7058.29    5495.40   67086.22
    Latency        7.08ms     3.89ms   357.28ms
    HTTP codes:
      1xx - 0, 2xx - 91027, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8973
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8973
    Throughput:     1.84MB/s
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
    Reqs/sec     24641.75    7663.35   35926.96
    Latency        2.03ms     2.09ms   184.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     21016.82    6047.59   29986.30
    Latency        2.38ms     2.15ms   193.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     63086.59    3437.55   65309.45
    Latency      790.69us    64.64us     2.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     19518.69    9408.48   82455.58
    Latency        2.56ms     2.49ms   218.47ms
    HTTP codes:
      1xx - 0, 2xx - 90998, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9002
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9002
    Throughput:     4.02MB/s
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
    Reqs/sec     27261.08    8283.49   59038.15
    Latency        1.83ms     1.96ms   168.78ms
    HTTP codes:
      1xx - 0, 2xx - 97571, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2429
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2429
    Throughput:     6.09MB/s
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
    Reqs/sec     75699.38    2692.46   83959.12
    Latency      657.56us   202.33us    13.44ms
    HTTP codes:
      1xx - 0, 2xx - 96328, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3672
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3672
    Throughput:    11.54MB/s
  ```


