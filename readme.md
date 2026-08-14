## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70790` | `4996` | `90394` |
| **82%** | [Hyper Express](#hyper-express) | `58323` | `3405` | `65738` |
| **29%** | [Hono](#hono) | `20858` | `6831` | `30220` |
| **29%** | [Fastify](#fastify) | `20742` | `5101` | `34935` |
| **29%** | [Node (Default)](#node-default) | `20641` | `5328` | `60669` |
| **27%** | [Koa](#koa) | `19205` | `7781` | `61841` |
| **10%** | [Carbon](#carbon) | `7343` | `1148` | `10273` |
| **9%** | [Express](#express) | `6157` | `1086` | `8307` |


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
    Reqs/sec      8253.58    7145.49   77824.68
    Latency        6.05ms     4.63ms   393.91ms
    HTTP codes:
      1xx - 0, 2xx - 87487, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12513
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12513
    Throughput:     1.64MB/s
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
    Reqs/sec      6190.82    1114.04    8235.05
    Latency        8.07ms     3.77ms   366.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     21275.82    6052.02   36520.91
    Latency        2.35ms     2.00ms   182.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     21813.02    6690.97   31104.40
    Latency        2.29ms     2.43ms   213.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.92MB/s
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
    Reqs/sec     58655.70    3391.19   65009.99
    Latency        0.85ms    91.91us     3.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.33MB/s
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
    Reqs/sec     20079.97    9317.05   73457.64
    Latency        2.49ms     2.42ms   211.09ms
    HTTP codes:
      1xx - 0, 2xx - 91057, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8943
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8943
    Throughput:     4.13MB/s
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
    Reqs/sec     20052.20    5556.19   71085.69
    Latency        2.49ms     2.09ms   178.75ms
    HTTP codes:
      1xx - 0, 2xx - 96422, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3578
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3578
    Throughput:     4.43MB/s
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
    Reqs/sec     70102.07    4243.95   85702.81
    Latency      709.26us   160.20us     8.11ms
    HTTP codes:
      1xx - 0, 2xx - 96709, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3291
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3291
    Throughput:    10.73MB/s
  ```


