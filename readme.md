## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `82011` | `3088` | `88468` |
| **81%** | [Hyper Express](#hyper-express) | `66099` | `6421` | `71224` |
| **43%** | [Node (Default)](#node-default) | `34927` | `10683` | `85595` |
| **38%** | [Koa](#koa) | `30959` | `12394` | `75138` |
| **37%** | [Fastify](#fastify) | `30254` | `8651` | `49586` |
| **35%** | [Hono](#hono) | `29108` | `8603` | `47863` |
| **11%** | [Carbon](#carbon) | `9156` | `2293` | `13223` |
| **9%** | [Express](#express) | `7334` | `1695` | `10529` |


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
    Reqs/sec      9747.95    5807.96   70345.16
    Latency        5.12ms     4.55ms   385.64ms
    HTTP codes:
      1xx - 0, 2xx - 93251, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6749
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6749
    Throughput:     2.06MB/s
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
    Reqs/sec      8551.42    7577.93   79217.66
    Latency        5.84ms     3.84ms   353.23ms
    HTTP codes:
      1xx - 0, 2xx - 87900, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12100
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12100
    Throughput:     2.15MB/s
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
    Reqs/sec     32303.42    8640.92   51498.94
    Latency        1.55ms     1.93ms   173.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.33MB/s
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
    Reqs/sec     28996.24    8400.68   46858.99
    Latency        1.72ms     2.11ms   184.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.55MB/s
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
    Reqs/sec     68464.62    3219.93   71525.79
    Latency      728.47us   121.29us     7.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.73MB/s
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
    Reqs/sec     28713.49   11448.42   82554.43
    Latency        1.74ms     2.24ms   192.76ms
    HTTP codes:
      1xx - 0, 2xx - 92388, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7612
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7612
    Throughput:     5.99MB/s
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
    Reqs/sec     36281.55   12370.02   84449.96
    Latency        1.38ms     1.88ms   160.16ms
    HTTP codes:
      1xx - 0, 2xx - 93767, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6233
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6233
    Throughput:     7.77MB/s
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
    Reqs/sec     78642.24    2683.90   84932.48
    Latency      633.70us   225.31us    12.96ms
    HTTP codes:
      1xx - 0, 2xx - 95309, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4691
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4691
    Throughput:    11.86MB/s
  ```


