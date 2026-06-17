## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79515` | `2915` | `83514` |
| **88%** | [Hyper Express](#hyper-express) | `69987` | `3733` | `74132` |
| **43%** | [Node (Default)](#node-default) | `33910` | `9509` | `78386` |
| **41%** | [Fastify](#fastify) | `32971` | `9396` | `52350` |
| **39%** | [Koa](#koa) | `31212` | `12936` | `79128` |
| **38%** | [Hono](#hono) | `29862` | `8959` | `47470` |
| **12%** | [Carbon](#carbon) | `9180` | `2346` | `13181` |
| **9%** | [Express](#express) | `7492` | `1766` | `10672` |


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
    Reqs/sec     10413.92    9160.94   89797.47
    Latency        4.79ms     4.39ms   378.03ms
    HTTP codes:
      1xx - 0, 2xx - 86749, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13251
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13251
    Throughput:     2.05MB/s
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
    Reqs/sec      8397.61    8270.57   80402.89
    Latency        5.95ms     3.89ms   357.88ms
    HTTP codes:
      1xx - 0, 2xx - 86393, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13607
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13607
    Throughput:     2.08MB/s
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
    Reqs/sec     33053.33   10416.65   52099.71
    Latency        1.51ms     1.88ms   170.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.51MB/s
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
    Reqs/sec     28915.14    9141.42   45990.59
    Latency        1.73ms     1.96ms   175.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.53MB/s
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
    Reqs/sec     69427.25    2621.25   72528.11
    Latency      718.33us    67.32us     3.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.86MB/s
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
    Reqs/sec     27926.77   11902.66   87589.43
    Latency        1.79ms     2.27ms   196.40ms
    HTTP codes:
      1xx - 0, 2xx - 92209, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7791
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7791
    Throughput:     5.82MB/s
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
    Reqs/sec     35775.55   11012.65   88928.76
    Latency        1.39ms     1.81ms   154.04ms
    HTTP codes:
      1xx - 0, 2xx - 94635, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5365
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5365
    Throughput:     7.77MB/s
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
    Reqs/sec     80141.92    2573.25   84660.97
    Latency      621.66us   193.54us    10.28ms
    HTTP codes:
      1xx - 0, 2xx - 96627, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3373
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3373
    Throughput:    12.26MB/s
  ```


