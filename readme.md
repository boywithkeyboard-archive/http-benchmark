## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74821` | `2848` | `82270` |
| **83%** | [Hyper Express](#hyper-express) | `62466` | `3239` | `65150` |
| **33%** | [Node (Default)](#node-default) | `24532` | `7614` | `65356` |
| **31%** | [Fastify](#fastify) | `22877` | `6347` | `35586` |
| **28%** | [Hono](#hono) | `20796` | `5785` | `29726` |
| **25%** | [Koa](#koa) | `18819` | `9096` | `81163` |
| **11%** | [Carbon](#carbon) | `8266` | `1467` | `10338` |
| **9%** | [Express](#express) | `6427` | `1058` | `8421` |


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
    Reqs/sec      8898.90    5597.59   75401.21
    Latency        5.61ms     4.26ms   369.00ms
    HTTP codes:
      1xx - 0, 2xx - 92591, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7409
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7409
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
    Reqs/sec      6491.31    1091.30    8352.57
    Latency        7.70ms     3.60ms   345.48ms
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
    Reqs/sec     24415.52    7353.84   36009.88
    Latency        2.05ms     1.94ms   174.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.53MB/s
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
    Reqs/sec     20732.08    5355.88   29077.31
    Latency        2.41ms     2.06ms   182.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     63325.85    3684.59   70402.70
    Latency      788.63us    73.54us     3.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     18934.25   10184.80   83595.20
    Latency        2.63ms     2.42ms   211.94ms
    HTTP codes:
      1xx - 0, 2xx - 89164, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10836
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10836
    Throughput:     3.82MB/s
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
    Reqs/sec     25431.61    7817.62   59622.87
    Latency        1.96ms     1.88ms   158.69ms
    HTTP codes:
      1xx - 0, 2xx - 97480, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2520
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2518
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:     5.67MB/s
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
    Reqs/sec     74867.74    2266.03   82175.43
    Latency      664.70us   173.81us     9.34ms
    HTTP codes:
      1xx - 0, 2xx - 96454, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3546
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3546
    Throughput:    11.43MB/s
  ```


