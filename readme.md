## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78557` | `3146` | `85508` |
| **88%** | [Hyper Express](#hyper-express) | `68880` | `3257` | `71760` |
| **45%** | [Node (Default)](#node-default) | `35196` | `9896` | `84080` |
| **41%** | [Fastify](#fastify) | `32086` | `9042` | `51665` |
| **39%** | [Koa](#koa) | `30460` | `12648` | `74231` |
| **38%** | [Hono](#hono) | `30223` | `9548` | `45393` |
| **12%** | [Carbon](#carbon) | `9793` | `2507` | `13766` |
| **10%** | [Express](#express) | `7580` | `1758` | `10890` |


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
    Reqs/sec     10081.12    6643.48   79292.26
    Latency        4.95ms     4.41ms   378.22ms
    HTTP codes:
      1xx - 0, 2xx - 91580, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8420
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8420
    Throughput:     2.10MB/s
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
    Reqs/sec      8432.80    7133.66   84234.03
    Latency        5.92ms     3.75ms   339.42ms
    HTTP codes:
      1xx - 0, 2xx - 89055, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10945
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10945
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
    Reqs/sec     33410.09   10272.55   51957.98
    Latency        1.50ms     2.03ms   177.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.57MB/s
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
    Reqs/sec     29152.73    8969.54   46581.00
    Latency        1.71ms     2.18ms   192.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.58MB/s
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
    Reqs/sec     69416.55    2605.02   72585.54
    Latency      718.52us    67.64us     2.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.86MB/s
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
    Reqs/sec     29758.54   12132.61   74071.70
    Latency        1.67ms     2.23ms   186.26ms
    HTTP codes:
      1xx - 0, 2xx - 92581, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7419
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7419
    Throughput:     6.23MB/s
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
    Reqs/sec     34471.46    9395.69   73655.54
    Latency        1.45ms     1.83ms   157.31ms
    HTTP codes:
      1xx - 0, 2xx - 96927, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3073
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3073
    Throughput:     7.65MB/s
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
    Reqs/sec     79619.80    2243.02   85277.65
    Latency      625.83us   224.50us    10.68ms
    HTTP codes:
      1xx - 0, 2xx - 93734, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6266
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6266
    Throughput:    11.81MB/s
  ```


