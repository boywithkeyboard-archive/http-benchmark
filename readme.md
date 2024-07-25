## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75807` | `7077` | `100248` |
| **86%** | [Hyper Express](#hyper-express) | `65021` | `4639` | `74017` |
| **36%** | [Node (Default)](#node-default) | `27060` | `7720` | `49582` |
| **34%** | [Fastify](#fastify) | `25813` | `7994` | `38749` |
| **28%** | [Hono](#hono) | `20934` | `5337` | `31500` |
| **25%** | [Koa](#koa) | `18945` | `6695` | `56928` |
| **11%** | [Carbon](#carbon) | `8660` | `1455` | `11112` |
| **9%** | [Express](#express) | `6893` | `1139` | `9098` |


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
    Reqs/sec      8962.46    3410.74   48526.80
    Latency        5.56ms     3.94ms   343.48ms
    HTTP codes:
      1xx - 0, 2xx - 95476, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4524
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4524
    Throughput:     1.95MB/s
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
    Reqs/sec      7316.55    3909.29   48332.17
    Latency        6.82ms     3.86ms   355.62ms
    HTTP codes:
      1xx - 0, 2xx - 93297, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6703
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6703
    Throughput:     1.95MB/s
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
    Reqs/sec     25386.07    7742.09   40081.27
    Latency        1.97ms     1.92ms   173.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.75MB/s
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
    Reqs/sec     21869.81    5950.86   31402.14
    Latency        2.28ms     1.90ms   173.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.94MB/s
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
    Reqs/sec     65948.23    4930.33   71773.71
    Latency      756.00us    79.35us     4.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.37MB/s
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
    Reqs/sec     19896.05    6826.91   53906.96
    Latency        2.51ms     2.25ms   198.15ms
    HTTP codes:
      1xx - 0, 2xx - 94747, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5253
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5253
    Throughput:     4.26MB/s
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
    Reqs/sec     27016.03    7666.53   59533.25
    Latency        1.84ms     1.80ms   152.55ms
    HTTP codes:
      1xx - 0, 2xx - 97393, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2607
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2607
    Throughput:     6.06MB/s
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
    Reqs/sec     76260.52    6629.31   89231.40
    Latency      653.93us   192.88us    11.78ms
    HTTP codes:
      1xx - 0, 2xx - 97718, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2282
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2282
    Throughput:    11.78MB/s
  ```


