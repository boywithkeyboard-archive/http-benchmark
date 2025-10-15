## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75365` | `2950` | `83770` |
| **83%** | [Hyper Express](#hyper-express) | `62808` | `3468` | `66236` |
| **36%** | [Node (Default)](#node-default) | `26851` | `9531` | `79759` |
| **31%** | [Fastify](#fastify) | `23110` | `6706` | `35183` |
| **28%** | [Hono](#hono) | `20946` | `5746` | `29276` |
| **25%** | [Koa](#koa) | `18679` | `7830` | `67066` |
| **11%** | [Carbon](#carbon) | `8280` | `1486` | `10580` |
| **8%** | [Express](#express) | `6344` | `1046` | `8318` |


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
    Reqs/sec      9202.60    7595.20   78012.73
    Latency        5.42ms     4.24ms   365.88ms
    HTTP codes:
      1xx - 0, 2xx - 88485, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11515
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11515
    Throughput:     1.85MB/s
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
    Reqs/sec      6949.31    5268.79   67601.52
    Latency        7.19ms     3.96ms   359.99ms
    HTTP codes:
      1xx - 0, 2xx - 91311, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8689
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8689
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
    Reqs/sec     23492.19    7298.60   36222.11
    Latency        2.13ms     1.97ms   178.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.33MB/s
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
    Reqs/sec     21313.32    6165.12   29642.79
    Latency        2.34ms     2.07ms   185.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     61833.57    3231.33   65283.39
    Latency      806.45us    68.41us     4.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.78MB/s
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
    Reqs/sec     19520.73    9620.98   83426.57
    Latency        2.55ms     2.46ms   214.95ms
    HTTP codes:
      1xx - 0, 2xx - 90682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9318
    Throughput:     4.01MB/s
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
    Reqs/sec     25120.07    7660.29   61126.09
    Latency        1.99ms     1.92ms   162.75ms
    HTTP codes:
      1xx - 0, 2xx - 97520, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2480
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2480
    Throughput:     5.60MB/s
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
    Reqs/sec     74258.22    2117.88   80056.86
    Latency      670.39us   166.80us    11.01ms
    HTTP codes:
      1xx - 0, 2xx - 96354, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3646
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3646
    Throughput:    11.33MB/s
  ```


