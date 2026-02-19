## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70968` | `3689` | `82419` |
| **83%** | [Hyper Express](#hyper-express) | `58605` | `4240` | `68375` |
| **30%** | [Node (Default)](#node-default) | `21313` | `5750` | `71243` |
| **30%** | [Hono](#hono) | `21304` | `6759` | `30845` |
| **30%** | [Fastify](#fastify) | `20952` | `5419` | `37684` |
| **25%** | [Koa](#koa) | `18055` | `7759` | `60320` |
| **11%** | [Carbon](#carbon) | `7931` | `1503` | `10092` |
| **9%** | [Express](#express) | `6133` | `1081` | `8297` |


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
    Reqs/sec      8599.15    6105.87   73475.46
    Latency        5.80ms     4.45ms   380.87ms
    HTTP codes:
      1xx - 0, 2xx - 91128, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8872
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8872
    Throughput:     1.78MB/s
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
    Reqs/sec      6043.40    1097.33    8231.84
    Latency        8.27ms     3.84ms   366.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     20305.49    4739.06   36847.84
    Latency        2.46ms     2.05ms   186.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     21786.04    7185.07   30621.44
    Latency        2.29ms     2.14ms   191.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.93MB/s
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
    Reqs/sec     58568.20    3569.84   65655.98
    Latency        0.85ms    85.37us     3.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.32MB/s
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
    Reqs/sec     18274.68    8393.74   69171.26
    Latency        2.73ms     2.53ms   224.96ms
    HTTP codes:
      1xx - 0, 2xx - 92225, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7775
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7775
    Throughput:     3.81MB/s
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
    Reqs/sec     20993.07    4953.71   59782.82
    Latency        2.38ms     2.00ms   174.85ms
    HTTP codes:
      1xx - 0, 2xx - 97477, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2523
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2523
    Throughput:     4.69MB/s
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
    Reqs/sec     69727.44    3554.43   85486.46
    Latency      714.29us   181.54us     9.69ms
    HTTP codes:
      1xx - 0, 2xx - 96489, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3511
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3511
    Throughput:    10.64MB/s
  ```


