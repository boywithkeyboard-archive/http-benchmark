## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75054` | `4564` | `106771` |
| **86%** | [Hyper Express](#hyper-express) | `64200` | `5829` | `80461` |
| **36%** | [Node (Default)](#node-default) | `26787` | `8668` | `60444` |
| **31%** | [Fastify](#fastify) | `23633` | `7324` | `35878` |
| **28%** | [Hono](#hono) | `21015` | `5906` | `30196` |
| **25%** | [Koa](#koa) | `18813` | `8385` | `73672` |
| **11%** | [Carbon](#carbon) | `8206` | `1442` | `10235` |
| **9%** | [Express](#express) | `6446` | `1081` | `8395` |


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
    Reqs/sec      8925.39    5778.03   67677.46
    Latency        5.59ms     4.25ms   370.64ms
    HTTP codes:
      1xx - 0, 2xx - 91802, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8198
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8198
    Throughput:     1.86MB/s
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
    Reqs/sec      7345.38    6729.64   83200.24
    Latency        6.79ms     3.90ms   358.36ms
    HTTP codes:
      1xx - 0, 2xx - 88513, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11487
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11487
    Throughput:     1.86MB/s
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
    Reqs/sec     24787.82    8064.59   37879.39
    Latency        2.02ms     2.10ms   188.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.61MB/s
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
    Reqs/sec     21578.58    6186.78   30571.36
    Latency        2.32ms     2.02ms   177.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     63511.06    3845.84   67925.65
    Latency      785.16us    74.19us     3.07ms
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
    Reqs/sec     17938.79    7445.11   66893.49
    Latency        2.78ms     2.38ms   207.98ms
    HTTP codes:
      1xx - 0, 2xx - 93023, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6977
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6977
    Throughput:     3.78MB/s
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
    Reqs/sec     26196.71    8480.29   70824.35
    Latency        1.90ms     1.88ms   161.94ms
    HTTP codes:
      1xx - 0, 2xx - 97055, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2945
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2945
    Throughput:     5.82MB/s
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
    Reqs/sec     75256.91    3298.94   83919.10
    Latency      662.63us   143.65us     7.40ms
    HTTP codes:
      1xx - 0, 2xx - 97418, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2582
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2582
    Throughput:    11.60MB/s
  ```


