## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73873` | `6141` | `77737` |
| **85%** | [Hyper Express](#hyper-express) | `62985` | `4127` | `75797` |
| **33%** | [Node (Default)](#node-default) | `24676` | `7128` | `40010` |
| **32%** | [Fastify](#fastify) | `23960` | `7411` | `35898` |
| **28%** | [Hono](#hono) | `20386` | `5967` | `29463` |
| **24%** | [Koa](#koa) | `18090` | `6195` | `52045` |
| **11%** | [Carbon](#carbon) | `7850` | `1259` | `10102` |
| **9%** | [Express](#express) | `6306` | `972` | `8363` |


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
    Reqs/sec      8537.70    4048.11   59820.13
    Latency        5.85ms     4.24ms   365.92ms
    HTTP codes:
      1xx - 0, 2xx - 94336, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5664
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5664
    Throughput:     1.83MB/s
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
    Reqs/sec      6367.04    1015.27    8223.75
    Latency        7.85ms     3.62ms   346.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24307.57    7781.22   36390.53
    Latency        2.06ms     2.09ms   183.49ms
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
    Reqs/sec     20203.17    5944.41   29904.95
    Latency        2.47ms     2.05ms   185.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.57MB/s
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
    Reqs/sec     63540.85    3097.09   72258.47
    Latency      784.76us    75.89us     3.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.02MB/s
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
    Reqs/sec     17784.80    6553.90   53222.37
    Latency        2.81ms     2.55ms   226.66ms
    HTTP codes:
      1xx - 0, 2xx - 94156, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5844
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5844
    Throughput:     3.79MB/s
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
    Reqs/sec     25836.33    7996.46   47232.79
    Latency        1.93ms     2.01ms   173.24ms
    HTTP codes:
      1xx - 0, 2xx - 98161, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1839
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1839
    Throughput:     5.81MB/s
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
    Reqs/sec     74284.02    7603.74   82645.00
    Latency      669.87us   255.42us    25.97ms
    HTTP codes:
      1xx - 0, 2xx - 97114, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2886
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2885
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:    11.42MB/s
  ```


