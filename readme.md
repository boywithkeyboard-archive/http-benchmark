## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73638` | `5147` | `81548` |
| **87%** | [Hyper Express](#hyper-express) | `64156` | `3715` | `68998` |
| **37%** | [Node (Default)](#node-default) | `26901` | `7776` | `43242` |
| **34%** | [Fastify](#fastify) | `24702` | `7813` | `36566` |
| **30%** | [Hono](#hono) | `21932` | `6079` | `30458` |
| **26%** | [Koa](#koa) | `19237` | `7933` | `62116` |
| **11%** | [Carbon](#carbon) | `8372` | `1428` | `10264` |
| **9%** | [Express](#express) | `6499` | `1047` | `8446` |


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
    Reqs/sec      8529.88    4203.36   54712.58
    Latency        5.85ms     4.26ms   372.18ms
    HTTP codes:
      1xx - 0, 2xx - 93846, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6154
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6154
    Throughput:     1.82MB/s
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
    Reqs/sec      6902.14    4280.50   51364.87
    Latency        7.24ms     3.93ms   360.57ms
    HTTP codes:
      1xx - 0, 2xx - 92180, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7820
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7820
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
    Reqs/sec     24531.71    7380.65   36253.71
    Latency        2.04ms     1.94ms   176.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     20314.01    5425.29   28812.68
    Latency        2.46ms     2.11ms   188.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     63708.70    4017.09   76957.08
    Latency      782.75us    73.93us     2.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     18731.26    6478.54   58580.69
    Latency        2.67ms     2.38ms   214.24ms
    HTTP codes:
      1xx - 0, 2xx - 94534, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5466
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5466
    Throughput:     4.00MB/s
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
    Reqs/sec     26294.50    8643.23   54213.68
    Latency        1.90ms     1.97ms   166.90ms
    HTTP codes:
      1xx - 0, 2xx - 96771, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3229
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3229
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
    Reqs/sec     74053.10    6121.84   88729.78
    Latency      672.61us   188.06us     9.81ms
    HTTP codes:
      1xx - 0, 2xx - 97834, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2166
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2166
    Throughput:    11.47MB/s
  ```


