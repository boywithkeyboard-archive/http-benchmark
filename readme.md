## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71969` | `3671` | `83624` |
| **82%** | [Hyper Express](#hyper-express) | `58765` | `3988` | `65459` |
| **30%** | [Hono](#hono) | `21609` | `6401` | `29022` |
| **29%** | [Fastify](#fastify) | `20963` | `5939` | `35068` |
| **27%** | [Node (Default)](#node-default) | `19587` | `4073` | `49102` |
| **26%** | [Koa](#koa) | `18420` | `8137` | `60392` |
| **10%** | [Carbon](#carbon) | `7331` | `1203` | `10179` |
| **8%** | [Express](#express) | `6072` | `1094` | `8330` |


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
    Reqs/sec      7859.21    5446.57   79311.60
    Latency        6.35ms     4.48ms   381.34ms
    HTTP codes:
      1xx - 0, 2xx - 92196, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7804
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7804
    Throughput:     1.65MB/s
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
    Reqs/sec      6247.39    1145.56    8360.36
    Latency        8.00ms     3.98ms   378.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     19909.49    5138.45   34728.15
    Latency        2.51ms     2.06ms   189.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.52MB/s
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
    Reqs/sec     21503.33    6366.39   30054.86
    Latency        2.32ms     2.14ms   190.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     59064.08    3355.31   70666.45
    Latency      844.47us    88.88us     3.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.39MB/s
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
    Reqs/sec     16449.79    7623.40   70547.94
    Latency        3.03ms     2.42ms   214.18ms
    HTTP codes:
      1xx - 0, 2xx - 92770, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7230
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7230
    Throughput:     3.45MB/s
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
    Reqs/sec     19837.20    5273.91   67461.99
    Latency        2.51ms     1.87ms   160.39ms
    HTTP codes:
      1xx - 0, 2xx - 96610, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3390
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3390
    Throughput:     4.39MB/s
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
    Reqs/sec     71721.49    4463.01   82949.09
    Latency      694.31us   232.24us    11.58ms
    HTTP codes:
      1xx - 0, 2xx - 96471, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3529
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3529
    Throughput:    10.95MB/s
  ```


