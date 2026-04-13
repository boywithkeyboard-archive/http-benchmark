## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `122565` | `6740` | `134743` |
| **76%** | [Hyper Express](#hyper-express) | `93172` | `6945` | `101311` |
| **30%** | [Node (Default)](#node-default) | `36361` | `10548` | `99044` |
| **28%** | [Fastify](#fastify) | `34331` | `7919` | `54169` |
| **26%** | [Koa](#koa) | `32147` | `17220` | `117574` |
| **25%** | [Hono](#hono) | `31145` | `7631` | `47505` |
| **10%** | [Carbon](#carbon) | `11789` | `2385` | `16274` |
| **7%** | [Express](#express) | `8527` | `1729` | `11809` |


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
    Reqs/sec     13293.96   11844.19  126688.61
    Latency        3.75ms     3.77ms   323.93ms
    HTTP codes:
      1xx - 0, 2xx - 86481, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13519
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13519
    Throughput:     2.61MB/s
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
    Reqs/sec      9920.07   12072.75  127973.47
    Latency        5.03ms     3.44ms   309.96ms
    HTTP codes:
      1xx - 0, 2xx - 83388, 3xx - 0, 4xx - 0, 5xx - 0
      others - 16612
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 16608
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 4
    Throughput:     2.37MB/s
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
    Reqs/sec     34999.37    8322.85   53880.04
    Latency        1.43ms     1.45ms   127.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.93MB/s
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
    Reqs/sec     30456.23    6972.36   46248.99
    Latency        1.64ms     1.53ms   134.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.88MB/s
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
    Reqs/sec     96830.95    7293.40  102937.57
    Latency      514.72us    57.56us     2.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    13.75MB/s
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
    Reqs/sec     31525.92   14374.26  107344.98
    Latency        1.58ms     1.87ms   156.95ms
    HTTP codes:
      1xx - 0, 2xx - 90184, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9816
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9816
    Throughput:     6.43MB/s
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
    Reqs/sec     37140.37   12166.78  105677.77
    Latency        1.34ms     1.42ms   118.08ms
    HTTP codes:
      1xx - 0, 2xx - 95529, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4471
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4471
    Throughput:     8.12MB/s
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
    Reqs/sec    122456.41    9720.71  161208.93
    Latency      408.82us   157.11us     6.28ms
    HTTP codes:
      1xx - 0, 2xx - 92708, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7292
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7292
    Throughput:    17.84MB/s
  ```


