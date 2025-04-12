## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75499` | `5271` | `88170` |
| **86%** | [Hyper Express](#hyper-express) | `65072` | `4688` | `77896` |
| **35%** | [Node (Default)](#node-default) | `26283` | `7950` | `45568` |
| **32%** | [Fastify](#fastify) | `24246` | `7380` | `36031` |
| **28%** | [Hono](#hono) | `21502` | `5974` | `31353` |
| **25%** | [Koa](#koa) | `19116` | `5909` | `43674` |
| **11%** | [Carbon](#carbon) | `8220` | `1307` | `10351` |
| **9%** | [Express](#express) | `6597` | `1111` | `8485` |


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
    Reqs/sec      8700.13    3608.94   46840.08
    Latency        5.74ms     4.29ms   368.83ms
    HTTP codes:
      1xx - 0, 2xx - 95167, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4833
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4833
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
    Reqs/sec      6579.68    1085.59    8408.66
    Latency        7.60ms     3.67ms   350.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     25245.13    8205.27   36541.28
    Latency        1.98ms     2.03ms   182.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.72MB/s
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
    Reqs/sec     21891.66    6013.66   30270.87
    Latency        2.28ms     1.98ms   175.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.95MB/s
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
    Reqs/sec     63281.26    4059.66   66551.73
    Latency      787.96us    75.17us     3.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.99MB/s
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
    Reqs/sec     19963.24    7700.63   57106.56
    Latency        2.50ms     2.38ms   211.14ms
    HTTP codes:
      1xx - 0, 2xx - 92148, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7852
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7852
    Throughput:     4.16MB/s
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
    Reqs/sec     25556.38    7465.55   60805.94
    Latency        1.95ms     1.87ms   163.65ms
    HTTP codes:
      1xx - 0, 2xx - 97379, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2621
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2621
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
    Reqs/sec     73963.39    6932.99   84633.02
    Latency      673.62us   237.95us    11.21ms
    HTTP codes:
      1xx - 0, 2xx - 97209, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2791
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2791
    Throughput:    11.37MB/s
  ```


