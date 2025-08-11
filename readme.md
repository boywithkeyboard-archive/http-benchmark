## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73542` | `3964` | `84866` |
| **85%** | [Hyper Express](#hyper-express) | `62840` | `3320` | `66147` |
| **36%** | [Node (Default)](#node-default) | `26223` | `8282` | `58681` |
| **33%** | [Fastify](#fastify) | `24562` | `7573` | `36353` |
| **29%** | [Hono](#hono) | `21002` | `5770` | `31227` |
| **26%** | [Koa](#koa) | `19105` | `7418` | `63750` |
| **11%** | [Carbon](#carbon) | `8374` | `1478` | `10550` |
| **9%** | [Express](#express) | `6503` | `1085` | `8523` |


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
    Reqs/sec      8959.29    4980.19   56803.30
    Latency        5.57ms     4.26ms   365.94ms
    HTTP codes:
      1xx - 0, 2xx - 92416, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7584
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7584
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
    Reqs/sec      7371.74    5410.80   65912.86
    Latency        6.76ms     3.87ms   351.57ms
    HTTP codes:
      1xx - 0, 2xx - 91171, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8829
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8829
    Throughput:     1.93MB/s
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
    Reqs/sec     23454.99    7283.53   37015.88
    Latency        2.13ms     2.00ms   181.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.32MB/s
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
    Reqs/sec     21424.89    6030.72   31172.23
    Latency        2.33ms     1.98ms   175.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.84MB/s
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
    Reqs/sec     62957.67    3285.41   68605.46
    Latency      791.83us    63.73us     2.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     19376.46    8005.30   65951.62
    Latency        2.57ms     2.36ms   208.44ms
    HTTP codes:
      1xx - 0, 2xx - 93162, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6838
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6838
    Throughput:     4.08MB/s
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
    Reqs/sec     26351.75    7800.83   61190.87
    Latency        1.89ms     1.88ms   160.29ms
    HTTP codes:
      1xx - 0, 2xx - 97464, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2536
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2536
    Throughput:     5.89MB/s
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
    Reqs/sec     74749.82    2500.48   83334.47
    Latency      666.67us   139.83us     5.36ms
    HTTP codes:
      1xx - 0, 2xx - 96999, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3001
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3001
    Throughput:    11.47MB/s
  ```


