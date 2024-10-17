## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74810` | `5390` | `85076` |
| **84%** | [Hyper Express](#hyper-express) | `63075` | `3225` | `71134` |
| **36%** | [Node (Default)](#node-default) | `26559` | `8017` | `44682` |
| **32%** | [Fastify](#fastify) | `23722` | `6750` | `36768` |
| **27%** | [Hono](#hono) | `20426` | `5589` | `30042` |
| **24%** | [Koa](#koa) | `17624` | `5725` | `46886` |
| **11%** | [Carbon](#carbon) | `8092` | `1425` | `10380` |
| **9%** | [Express](#express) | `6492` | `1020` | `8359` |


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
    Reqs/sec      8610.21    3121.26   42465.84
    Latency        5.80ms     4.14ms   361.34ms
    HTTP codes:
      1xx - 0, 2xx - 96102, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3898
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3898
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
    Reqs/sec      6547.34    1046.25    8434.97
    Latency        7.63ms     3.65ms   348.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24589.61    7597.55   36293.43
    Latency        2.03ms     1.93ms   177.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.58MB/s
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
    Reqs/sec     21712.54    6391.45   30913.55
    Latency        2.30ms     1.91ms   171.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     62657.76    4284.97   77396.21
    Latency      796.00us    74.50us     3.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     18721.40    7030.50   54161.48
    Latency        2.67ms     2.32ms   203.46ms
    HTTP codes:
      1xx - 0, 2xx - 93730, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6270
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6270
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
    Reqs/sec     24978.07    7137.38   50590.68
    Latency        2.00ms     1.90ms   171.77ms
    HTTP codes:
      1xx - 0, 2xx - 97791, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2209
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2209
    Throughput:     5.58MB/s
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
    Reqs/sec     74440.97    6117.30   92315.99
    Latency      670.25us   162.45us     7.39ms
    HTTP codes:
      1xx - 0, 2xx - 97269, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2731
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2731
    Throughput:    11.44MB/s
  ```


