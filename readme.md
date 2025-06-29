## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73379` | `4903` | `82433` |
| **84%** | [Hyper Express](#hyper-express) | `61932` | `3215` | `65606` |
| **35%** | [Node (Default)](#node-default) | `25845` | `7747` | `52364` |
| **32%** | [Fastify](#fastify) | `23549` | `6825` | `35915` |
| **30%** | [Hono](#hono) | `22077` | `6139` | `30643` |
| **26%** | [Koa](#koa) | `19423` | `7199` | `58557` |
| **11%** | [Carbon](#carbon) | `8395` | `1408` | `10384` |
| **9%** | [Express](#express) | `6520` | `1089` | `8415` |


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
    Reqs/sec      8687.43    4316.36   54188.47
    Latency        5.73ms     4.23ms   365.84ms
    HTTP codes:
      1xx - 0, 2xx - 93469, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6531
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6531
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
    Reqs/sec      6599.94    1118.07    8467.48
    Latency        7.57ms     3.68ms   351.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     24597.50    7006.63   37312.74
    Latency        2.03ms     1.96ms   177.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     21103.04    6202.65   29946.33
    Latency        2.37ms     2.03ms   181.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     61919.03    3893.37   65345.74
    Latency      805.37us    78.87us     3.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     19367.61    6924.36   56165.37
    Latency        2.58ms     2.34ms   206.64ms
    HTTP codes:
      1xx - 0, 2xx - 94170, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5830
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5830
    Throughput:     4.12MB/s
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
    Reqs/sec     25747.79    7512.66   49064.30
    Latency        1.94ms     1.93ms   168.87ms
    HTTP codes:
      1xx - 0, 2xx - 98031, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1969
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1969
    Throughput:     5.77MB/s
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
    Reqs/sec     73083.13    4874.80   82413.03
    Latency      681.89us   183.08us     8.48ms
    HTTP codes:
      1xx - 0, 2xx - 97064, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2936
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2936
    Throughput:    11.23MB/s
  ```


