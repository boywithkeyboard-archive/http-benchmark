## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `81318` | `2858` | `86920` |
| **87%** | [Hyper Express](#hyper-express) | `70986` | `2756` | `74721` |
| **47%** | [Node (Default)](#node-default) | `37893` | `12154` | `72842` |
| **41%** | [Fastify](#fastify) | `33358` | `10243` | `52123` |
| **38%** | [Koa](#koa) | `31284` | `12760` | `74463` |
| **37%** | [Hono](#hono) | `30113` | `9148` | `47753` |
| **12%** | [Carbon](#carbon) | `9491` | `2351` | `13425` |
| **9%** | [Express](#express) | `7563` | `1826` | `10899` |


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
    Reqs/sec     10065.46    7110.52   77444.34
    Latency        4.96ms     4.17ms   361.01ms
    HTTP codes:
      1xx - 0, 2xx - 90649, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9351
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9351
    Throughput:     2.07MB/s
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
    Reqs/sec      8156.70    7240.28   82462.70
    Latency        6.12ms     3.85ms   350.49ms
    HTTP codes:
      1xx - 0, 2xx - 88448, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11552
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11552
    Throughput:     2.07MB/s
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
    Reqs/sec     33029.97    9800.93   53343.64
    Latency        1.51ms     1.72ms   152.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.49MB/s
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
    Reqs/sec     29832.17    8901.76   47914.10
    Latency        1.67ms     1.87ms   165.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.74MB/s
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
    Reqs/sec     70994.01    3540.02   74034.04
    Latency      702.49us    68.57us     2.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.08MB/s
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
    Reqs/sec     29623.52   12075.89   79106.43
    Latency        1.68ms     2.22ms   193.15ms
    HTTP codes:
      1xx - 0, 2xx - 92652, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7348
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7348
    Throughput:     6.21MB/s
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
    Reqs/sec     36285.62   11104.02   85132.90
    Latency        1.37ms     1.77ms   149.04ms
    HTTP codes:
      1xx - 0, 2xx - 95127, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4873
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4873
    Throughput:     7.91MB/s
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
    Reqs/sec     80755.55    3790.86   85473.54
    Latency      617.16us   141.31us     5.67ms
    HTTP codes:
      1xx - 0, 2xx - 96963, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3037
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3037
    Throughput:    12.39MB/s
  ```


