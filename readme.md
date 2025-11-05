## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74870` | `2572` | `80209` |
| **84%** | [Hyper Express](#hyper-express) | `62788` | `4916` | `78391` |
| **33%** | [Node (Default)](#node-default) | `24995` | `6966` | `63750` |
| **32%** | [Fastify](#fastify) | `23852` | `7388` | `35714` |
| **27%** | [Hono](#hono) | `20498` | `5495` | `30326` |
| **24%** | [Koa](#koa) | `18217` | `8692` | `75637` |
| **11%** | [Carbon](#carbon) | `8183` | `1436` | `10323` |
| **8%** | [Express](#express) | `6356` | `1035` | `8448` |


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
    Reqs/sec      9146.99    6611.84   78000.94
    Latency        5.46ms     4.25ms   364.42ms
    HTTP codes:
      1xx - 0, 2xx - 90778, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9222
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9222
    Throughput:     1.89MB/s
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
    Reqs/sec      6907.05    5131.47   65907.60
    Latency        7.23ms     3.94ms   358.93ms
    HTTP codes:
      1xx - 0, 2xx - 91897, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8103
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8103
    Throughput:     1.82MB/s
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
    Reqs/sec     23909.91    7536.43   37312.09
    Latency        2.09ms     2.06ms   184.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     21212.81    5867.98   30134.05
    Latency        2.36ms     1.95ms   176.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     62471.45    3740.34   66343.81
    Latency      798.21us    76.39us     3.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     18682.68    8261.12   65679.49
    Latency        2.67ms     2.45ms   213.48ms
    HTTP codes:
      1xx - 0, 2xx - 92434, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7566
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7566
    Throughput:     3.91MB/s
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
    Reqs/sec     25589.43    8216.59   69270.64
    Latency        1.95ms     1.92ms   167.76ms
    HTTP codes:
      1xx - 0, 2xx - 96175, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3825
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3825
    Throughput:     5.64MB/s
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
    Reqs/sec     74395.43    2437.70   77745.48
    Latency      669.76us   145.33us     6.33ms
    HTTP codes:
      1xx - 0, 2xx - 97108, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2892
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2892
    Throughput:    11.44MB/s
  ```


