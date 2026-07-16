## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `147983` | `8859` | `158626` |
| **85%** | [Hyper Express](#hyper-express) | `125694` | `11734` | `136402` |
| **35%** | [Node (Default)](#node-default) | `52090` | `13447` | `133680` |
| **34%** | [Fastify](#fastify) | `50788` | `10005` | `67615` |
| **29%** | [Hono](#hono) | `42626` | `8661` | `64812` |
| **29%** | [Koa](#koa) | `42554` | `21580` | `147040` |
| **13%** | [Carbon](#carbon) | `18648` | `5089` | `27280` |
| **9%** | [Express](#express) | `12900` | `2610` | `18857` |


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
    Reqs/sec     19417.24   13637.14  138540.67
    Latency        2.57ms     3.64ms   291.75ms
    HTTP codes:
      1xx - 0, 2xx - 90222, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9778
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9778
    Throughput:     3.98MB/s
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
    Reqs/sec     15098.16   14828.97  146671.12
    Latency        3.30ms     2.48ms   225.54ms
    HTTP codes:
      1xx - 0, 2xx - 85637, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14363
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14363
    Throughput:     3.71MB/s
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
    Reqs/sec     58203.88   24897.38  137208.06
    Latency        0.85ms     1.14ms    88.00ms
    HTTP codes:
      1xx - 0, 2xx - 73751, 3xx - 0, 4xx - 0, 5xx - 0
      others - 26249
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 26249
    Throughput:     9.76MB/s
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
    Reqs/sec     47098.19   19975.76  145928.19
    Latency        1.06ms     1.18ms    95.07ms
    HTTP codes:
      1xx - 0, 2xx - 89103, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10897
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10897
    Throughput:     9.47MB/s
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
    Reqs/sec    126125.95   11019.20  135047.59
    Latency      393.41us   168.98us     7.13ms
    HTTP codes:
      1xx - 0, 2xx - 90737, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9263
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9263
    Throughput:    16.25MB/s
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
    Reqs/sec     42364.73   20694.54  152876.05
    Latency        1.18ms     1.23ms   100.55ms
    HTTP codes:
      1xx - 0, 2xx - 86581, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13419
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13419
    Throughput:     8.29MB/s
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
    Reqs/sec     53466.34   13197.44  121431.17
    Latency        0.93ms     0.91ms    70.45ms
    HTTP codes:
      1xx - 0, 2xx - 95160, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4840
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4840
    Throughput:    11.66MB/s
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
    Reqs/sec    146548.14    9144.60  160156.98
    Latency      339.19us   131.37us     8.44ms
    HTTP codes:
      1xx - 0, 2xx - 93797, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6203
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6203
    Throughput:    21.71MB/s
  ```


