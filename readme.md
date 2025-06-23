## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73803` | `4230` | `80590` |
| **83%** | [Hyper Express](#hyper-express) | `60997` | `4675` | `76244` |
| **36%** | [Node (Default)](#node-default) | `26278` | `8270` | `48758` |
| **34%** | [Fastify](#fastify) | `24761` | `7953` | `37357` |
| **28%** | [Hono](#hono) | `20697` | `5941` | `30296` |
| **25%** | [Koa](#koa) | `18689` | `5874` | `39242` |
| **11%** | [Carbon](#carbon) | `8157` | `1399` | `10212` |
| **9%** | [Express](#express) | `6340` | `1007` | `8334` |


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
    Reqs/sec      8552.75    4588.14   56282.99
    Latency        5.83ms     4.33ms   374.87ms
    HTTP codes:
      1xx - 0, 2xx - 93136, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6864
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6864
    Throughput:     1.81MB/s
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
    Reqs/sec      6493.17    1089.54    8351.49
    Latency        7.70ms     3.48ms   339.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     24161.84    7548.76   36246.95
    Latency        2.07ms     2.01ms   179.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     21016.78    6308.98   30640.09
    Latency        2.38ms     2.10ms   187.03ms
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
    Reqs/sec     61300.12    2786.11   63729.00
    Latency      813.10us    70.67us     3.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     18516.32    6126.33   49261.56
    Latency        2.69ms     2.39ms   209.93ms
    HTTP codes:
      1xx - 0, 2xx - 94718, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5282
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5282
    Throughput:     3.97MB/s
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
    Reqs/sec     25651.97    7643.03   57509.56
    Latency        1.95ms     2.00ms   170.06ms
    HTTP codes:
      1xx - 0, 2xx - 96981, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3019
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3019
    Throughput:     5.70MB/s
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
    Reqs/sec     74184.10    5665.22   89466.21
    Latency      671.60us   168.93us     7.67ms
    HTTP codes:
      1xx - 0, 2xx - 98040, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1960
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1960
    Throughput:    11.51MB/s
  ```


