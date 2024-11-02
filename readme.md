## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72769` | `6085` | `83783` |
| **85%** | [Hyper Express](#hyper-express) | `62002` | `2375` | `65372` |
| **36%** | [Node (Default)](#node-default) | `25940` | `7661` | `57635` |
| **32%** | [Fastify](#fastify) | `23228` | `6640` | `34860` |
| **28%** | [Hono](#hono) | `20221` | `5314` | `29207` |
| **25%** | [Koa](#koa) | `17886` | `5889` | `46141` |
| **11%** | [Carbon](#carbon) | `8256` | `1406` | `10537` |
| **9%** | [Express](#express) | `6678` | `1100` | `8532` |


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
    Reqs/sec      8771.42    4186.88   57745.02
    Latency        5.69ms     4.21ms   365.49ms
    HTTP codes:
      1xx - 0, 2xx - 93748, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6252
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6252
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
    Reqs/sec      6534.45    1012.75    8676.11
    Latency        7.65ms     3.52ms   340.31ms
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
    Reqs/sec     23971.07    6961.74   35756.58
    Latency        2.08ms     2.10ms   182.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.44MB/s
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
    Reqs/sec     20660.78    5462.62   28905.41
    Latency        2.42ms     1.97ms   178.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     62554.80    3736.06   68743.59
    Latency      796.76us    68.12us     3.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     18159.16    6373.25   54857.30
    Latency        2.75ms     2.32ms   205.75ms
    HTTP codes:
      1xx - 0, 2xx - 94516, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5484
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5484
    Throughput:     3.88MB/s
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
    Reqs/sec     25562.04    7493.02   41803.50
    Latency        1.95ms     1.80ms   154.14ms
    HTTP codes:
      1xx - 0, 2xx - 98174, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1826
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1826
    Throughput:     5.75MB/s
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
    Reqs/sec     72881.30    5808.89   77662.99
    Latency      684.00us   189.29us    20.73ms
    HTTP codes:
      1xx - 0, 2xx - 97989, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2011
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2011
    Throughput:    11.29MB/s
  ```


