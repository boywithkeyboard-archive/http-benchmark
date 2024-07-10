## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74787` | `6054` | `84471` |
| **84%** | [Hyper Express](#hyper-express) | `63040` | `3853` | `67038` |
| **34%** | [Node (Default)](#node-default) | `25231` | `7187` | `56084` |
| **32%** | [Fastify](#fastify) | `23848` | `7809` | `36847` |
| **27%** | [Hono](#hono) | `20471` | `5548` | `29350` |
| **23%** | [Koa](#koa) | `16870` | `5697` | `45234` |
| **11%** | [Carbon](#carbon) | `7936` | `1334` | `10187` |
| **9%** | [Express](#express) | `6394` | `1023` | `8336` |


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
    Reqs/sec      8152.80    3622.47   55159.63
    Latency        6.13ms     4.24ms   370.44ms
    HTTP codes:
      1xx - 0, 2xx - 95078, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4922
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4922
    Throughput:     1.76MB/s
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
    Reqs/sec      6336.06    1006.17    8267.56
    Latency        7.89ms     3.66ms   350.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     22537.30    6182.35   34111.32
    Latency        2.22ms     2.04ms   183.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.10MB/s
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
    Reqs/sec     20297.20    5863.56   29336.51
    Latency        2.46ms     2.11ms   191.36ms
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
    Reqs/sec     62028.97    3346.87   66974.10
    Latency      803.72us    65.96us     4.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     18131.51    6684.74   52304.89
    Latency        2.75ms     2.39ms   210.87ms
    HTTP codes:
      1xx - 0, 2xx - 93325, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6675
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6675
    Throughput:     3.83MB/s
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
    Reqs/sec     24551.78    6932.33   43194.57
    Latency        2.03ms     1.93ms   163.41ms
    HTTP codes:
      1xx - 0, 2xx - 98306, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1694
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1694
    Throughput:     5.52MB/s
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
    Reqs/sec     73718.35    6296.65   83380.08
    Latency      674.95us   263.23us    17.54ms
    HTTP codes:
      1xx - 0, 2xx - 97138, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2862
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2862
    Throughput:    11.33MB/s
  ```


