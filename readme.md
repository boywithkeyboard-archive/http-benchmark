## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `125600` | `4343` | `133451` |
| **79%** | [Hyper Express](#hyper-express) | `99566` | `6503` | `104079` |
| **30%** | [Node (Default)](#node-default) | `37505` | `10396` | `100430` |
| **28%** | [Fastify](#fastify) | `35512` | `9016` | `56619` |
| **26%** | [Koa](#koa) | `32338` | `14577` | `107252` |
| **24%** | [Hono](#hono) | `30418` | `7734` | `49544` |
| **10%** | [Carbon](#carbon) | `12110` | `2500` | `16431` |
| **7%** | [Express](#express) | `8586` | `1611` | `11624` |


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
    Reqs/sec     13666.47   11092.41  115571.65
    Latency        3.65ms     3.61ms   312.64ms
    HTTP codes:
      1xx - 0, 2xx - 88765, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11235
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11235
    Throughput:     2.76MB/s
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
    Reqs/sec      9920.59   10180.77  109833.44
    Latency        5.03ms     3.20ms   293.92ms
    HTTP codes:
      1xx - 0, 2xx - 87690, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12310
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12310
    Throughput:     2.49MB/s
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
    Reqs/sec     36038.98    9229.46   56982.49
    Latency        1.39ms     1.43ms   126.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.18MB/s
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
    Reqs/sec     32089.67    7697.96   49799.40
    Latency        1.56ms     1.41ms   125.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.25MB/s
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
    Reqs/sec    100866.45    6920.18  104814.94
    Latency      494.10us    41.45us     2.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.32MB/s
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
    Reqs/sec     32598.36   15368.80  110622.73
    Latency        1.53ms     1.82ms   154.00ms
    HTTP codes:
      1xx - 0, 2xx - 89452, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10548
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10548
    Throughput:     6.59MB/s
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
    Reqs/sec     37350.77   10231.96  101474.51
    Latency        1.34ms     1.40ms   115.15ms
    HTTP codes:
      1xx - 0, 2xx - 96509, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3491
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3491
    Throughput:     8.26MB/s
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
    Reqs/sec    125075.62    6292.34  129246.20
    Latency      396.72us   136.67us     7.22ms
    HTTP codes:
      1xx - 0, 2xx - 96599, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3401
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3401
    Throughput:    19.11MB/s
  ```


