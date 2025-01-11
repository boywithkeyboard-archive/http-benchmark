## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74439` | `5067` | `79956` |
| **86%** | [Hyper Express](#hyper-express) | `64376` | `4446` | `79816` |
| **35%** | [Node (Default)](#node-default) | `25960` | `7475` | `44951` |
| **32%** | [Fastify](#fastify) | `24157` | `7254` | `35554` |
| **29%** | [Hono](#hono) | `21464` | `6007` | `31107` |
| **25%** | [Koa](#koa) | `18939` | `7290` | `62071` |
| **11%** | [Carbon](#carbon) | `8383` | `1404` | `10759` |
| **9%** | [Express](#express) | `6644` | `1060` | `8676` |


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
    Reqs/sec      8815.83    3726.29   50947.99
    Latency        5.66ms     4.10ms   355.50ms
    HTTP codes:
      1xx - 0, 2xx - 95125, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4875
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4875
    Throughput:     1.91MB/s
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
    Reqs/sec      7385.97    5630.73   57017.24
    Latency        6.77ms     3.89ms   353.48ms
    HTTP codes:
      1xx - 0, 2xx - 88710, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11290
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11281
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 9
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
    Reqs/sec     24276.32    7389.07   35959.46
    Latency        2.06ms     1.86ms   170.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     21077.61    6151.34   30871.94
    Latency        2.37ms     1.99ms   178.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     62708.34    2636.95   65129.94
    Latency      795.07us    71.17us     3.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     17857.28    5813.25   45164.08
    Latency        2.79ms     2.22ms   195.06ms
    HTTP codes:
      1xx - 0, 2xx - 95085, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4915
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4915
    Throughput:     3.84MB/s
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
    Reqs/sec     26983.57    8720.11   64955.75
    Latency        1.84ms     1.83ms   154.40ms
    HTTP codes:
      1xx - 0, 2xx - 96966, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3034
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3034
    Throughput:     5.99MB/s
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
    Reqs/sec     74912.67    7274.85  108431.72
    Latency      667.82us   227.46us    21.66ms
    HTTP codes:
      1xx - 0, 2xx - 96547, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3453
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3453
    Throughput:    11.38MB/s
  ```


