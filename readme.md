## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75000` | `5505` | `85263` |
| **85%** | [Hyper Express](#hyper-express) | `63421` | `3620` | `68586` |
| **36%** | [Node (Default)](#node-default) | `26920` | `7779` | `56473` |
| **33%** | [Fastify](#fastify) | `24431` | `7162` | `36817` |
| **28%** | [Hono](#hono) | `21070` | `5825` | `30712` |
| **25%** | [Koa](#koa) | `19116` | `7197` | `56165` |
| **11%** | [Carbon](#carbon) | `8357` | `1457` | `10533` |
| **9%** | [Express](#express) | `6617` | `1105` | `8624` |


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
    Reqs/sec      8906.33    4799.71   57322.88
    Latency        5.60ms     4.23ms   368.96ms
    HTTP codes:
      1xx - 0, 2xx - 93016, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6984
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6984
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
    Reqs/sec      7274.85    4918.48   55545.92
    Latency        6.87ms     3.89ms   353.09ms
    HTTP codes:
      1xx - 0, 2xx - 90759, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9241
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9241
    Throughput:     1.89MB/s
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
    Reqs/sec     25104.38    7572.33   36427.55
    Latency        1.99ms     2.04ms   183.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.69MB/s
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
    Reqs/sec     21304.93    5923.69   30861.78
    Latency        2.34ms     1.99ms   181.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.81MB/s
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
    Reqs/sec     63306.77    3612.09   65848.98
    Latency      787.67us    64.60us     3.36ms
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
    Reqs/sec     18953.91    6413.80   49994.56
    Latency        2.63ms     2.34ms   205.38ms
    HTTP codes:
      1xx - 0, 2xx - 94571, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5429
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5429
    Throughput:     4.05MB/s
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
    Reqs/sec     27803.64    8678.07   52931.38
    Latency        1.80ms     2.00ms   172.03ms
    HTTP codes:
      1xx - 0, 2xx - 97533, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2467
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2467
    Throughput:     6.21MB/s
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
    Reqs/sec     74433.95    5438.35   89685.70
    Latency      668.83us   157.60us     8.22ms
    HTTP codes:
      1xx - 0, 2xx - 97108, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2892
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2892
    Throughput:    11.44MB/s
  ```


