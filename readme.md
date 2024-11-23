## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74027` | `5886` | `86744` |
| **85%** | [Hyper Express](#hyper-express) | `62828` | `3685` | `66399` |
| **35%** | [Node (Default)](#node-default) | `26132` | `7723` | `52852` |
| **32%** | [Fastify](#fastify) | `23697` | `6921` | `35577` |
| **29%** | [Hono](#hono) | `21692` | `6057` | `29748` |
| **25%** | [Koa](#koa) | `18365` | `5719` | `41301` |
| **11%** | [Carbon](#carbon) | `8423` | `1481` | `10558` |
| **9%** | [Express](#express) | `6623` | `1077` | `8450` |


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
    Reqs/sec      8656.89    3636.53   50268.82
    Latency        5.77ms     4.12ms   359.21ms
    HTTP codes:
      1xx - 0, 2xx - 95504, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4496
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4496
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
    Reqs/sec      6635.71    1105.64    8612.13
    Latency        7.53ms     3.50ms   334.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.90MB/s
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
    Reqs/sec     24665.62    7489.10   35575.00
    Latency        2.03ms     2.06ms   183.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     20679.58    5801.82   29653.18
    Latency        2.42ms     2.03ms   181.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     63150.14    4085.53   69002.11
    Latency      789.75us    69.24us     4.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     18605.87    5994.34   44435.60
    Latency        2.68ms     2.30ms   206.12ms
    HTTP codes:
      1xx - 0, 2xx - 95250, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4750
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4750
    Throughput:     4.01MB/s
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
    Reqs/sec     27076.18    7841.46   41025.91
    Latency        1.84ms     1.92ms   164.42ms
    HTTP codes:
      1xx - 0, 2xx - 98256, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1744
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1744
    Throughput:     6.10MB/s
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
    Reqs/sec     74243.09    4981.32   81176.66
    Latency      671.47us   167.90us    11.78ms
    HTTP codes:
      1xx - 0, 2xx - 97397, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2603
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2603
    Throughput:    11.44MB/s
  ```


