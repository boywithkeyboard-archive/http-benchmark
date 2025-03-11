## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74282` | `5002` | `84842` |
| **84%** | [Hyper Express](#hyper-express) | `62538` | `3622` | `72803` |
| **34%** | [Node (Default)](#node-default) | `25176` | `7165` | `49922` |
| **33%** | [Fastify](#fastify) | `24854` | `7617` | `37242` |
| **27%** | [Hono](#hono) | `20036` | `4951` | `30096` |
| **25%** | [Koa](#koa) | `18601` | `7234` | `61111` |
| **11%** | [Carbon](#carbon) | `8405` | `1444` | `10519` |
| **9%** | [Express](#express) | `6560` | `1071` | `8572` |


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
    Reqs/sec      8741.61    4827.55   56807.05
    Latency        5.71ms     4.22ms   367.43ms
    HTTP codes:
      1xx - 0, 2xx - 92947, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7053
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7053
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
    Reqs/sec      6557.44    1091.70    8594.19
    Latency        7.62ms     3.67ms   345.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23950.14    6840.39   36354.02
    Latency        2.09ms     2.01ms   180.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     20279.75    5601.19   30559.90
    Latency        2.46ms     1.99ms   181.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     63944.73    3787.96   69109.43
    Latency      780.50us    65.00us     2.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.08MB/s
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
    Reqs/sec     18138.69    5882.07   55912.78
    Latency        2.75ms     2.38ms   206.57ms
    HTTP codes:
      1xx - 0, 2xx - 94860, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5140
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5140
    Throughput:     3.89MB/s
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
    Reqs/sec     26674.33    8203.24   52223.89
    Latency        1.87ms     1.85ms   157.51ms
    HTTP codes:
      1xx - 0, 2xx - 97536, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2464
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2464
    Throughput:     5.96MB/s
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
    Reqs/sec     73525.36    6104.32   87336.38
    Latency      677.11us   148.51us     8.01ms
    HTTP codes:
      1xx - 0, 2xx - 98177, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1823
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1823
    Throughput:    11.42MB/s
  ```


