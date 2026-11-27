## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `100055` | `6942` | `132499` |
| **85%** | [Hyper Express](#hyper-express) | `85543` | `4005` | `88773` |
| **48%** | [Node (Default)](#node-default) | `47654` | `14785` | `90067` |
| **46%** | [Fastify](#fastify) | `46267` | `14879` | `66566` |
| **42%** | [Hono](#hono) | `42316` | `14372` | `59788` |
| **41%** | [Koa](#koa) | `41425` | `16244` | `103102` |
| **11%** | [Carbon](#carbon) | `11332` | `2447` | `17114` |
| **9%** | [Express](#express) | `9163` | `1847` | `13218` |


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
    Reqs/sec     12100.14    8148.23   95750.16
    Latency        4.12ms     3.43ms   291.05ms
    HTTP codes:
      1xx - 0, 2xx - 91887, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8113
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8113
    Throughput:     2.53MB/s
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
    Reqs/sec     10431.77    7652.48   94430.64
    Latency        4.78ms     2.88ms   264.51ms
    HTTP codes:
      1xx - 0, 2xx - 91326, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8674
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8673
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     2.73MB/s
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
    Reqs/sec     44954.41   16007.75   65824.45
    Latency        1.11ms     1.51ms   134.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.20MB/s
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
    Reqs/sec     40685.59   13960.94   59097.78
    Latency        1.23ms     1.52ms   134.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.16MB/s
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
    Reqs/sec     86158.11    3823.43   90129.74
    Latency      578.87us    55.99us     2.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.23MB/s
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
    Reqs/sec     37964.01   16072.29  106503.47
    Latency        1.31ms     1.73ms   148.93ms
    HTTP codes:
      1xx - 0, 2xx - 89933, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10067
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10067
    Throughput:     7.73MB/s
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
    Reqs/sec     53098.66   16689.74  102864.51
    Latency        0.94ms     1.30ms   104.77ms
    HTTP codes:
      1xx - 0, 2xx - 95310, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4690
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4690
    Throughput:    11.57MB/s
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
    Reqs/sec    100608.25    2770.21  106465.95
    Latency      494.33us   176.59us    11.30ms
    HTTP codes:
      1xx - 0, 2xx - 96084, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3916
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3916
    Throughput:    15.30MB/s
  ```


