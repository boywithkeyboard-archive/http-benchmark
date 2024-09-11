## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73690` | `5503` | `79059` |
| **85%** | [Hyper Express](#hyper-express) | `62370` | `3258` | `68329` |
| **36%** | [Node (Default)](#node-default) | `26222` | `8170` | `65058` |
| **33%** | [Fastify](#fastify) | `24658` | `7511` | `36850` |
| **29%** | [Hono](#hono) | `21049` | `5959` | `29883` |
| **24%** | [Koa](#koa) | `17621` | `5804` | `49343` |
| **11%** | [Carbon](#carbon) | `8253` | `1367` | `10568` |
| **9%** | [Express](#express) | `6461` | `1030` | `8452` |


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
    Reqs/sec      8761.86    4382.22   57933.26
    Latency        5.70ms     4.14ms   367.43ms
    HTTP codes:
      1xx - 0, 2xx - 93807, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6193
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6193
    Throughput:     1.87MB/s
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
    Reqs/sec      6518.48    1066.79    8518.80
    Latency        7.67ms     3.64ms   349.06ms
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
    Reqs/sec     24199.40    7380.04   35704.56
    Latency        2.06ms     2.09ms   188.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     20760.91    5760.19   29329.16
    Latency        2.41ms     1.99ms   179.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     62005.60    2923.81   69706.42
    Latency      804.11us    75.59us     3.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     18466.41    6531.57   57970.90
    Latency        2.70ms     2.27ms   198.39ms
    HTTP codes:
      1xx - 0, 2xx - 93790, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6210
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6210
    Throughput:     3.92MB/s
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
    Reqs/sec     25159.90    7003.45   59580.67
    Latency        1.98ms     1.88ms   163.02ms
    HTTP codes:
      1xx - 0, 2xx - 97674, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2326
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2326
    Throughput:     5.62MB/s
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
    Reqs/sec     74220.90    5575.08   84840.18
    Latency      671.56us   185.44us     7.87ms
    HTTP codes:
      1xx - 0, 2xx - 97651, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2349
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2349
    Throughput:    11.47MB/s
  ```


